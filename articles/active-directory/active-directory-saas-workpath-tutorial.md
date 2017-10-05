---
title: "Självstudier: Azure Active Directory-integrering med Workpath | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Workpath."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 320b0daf-14be-4813-b59b-25a6a5070690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: f4efa56d2c0374a977c1e46dad64b596cc9c3ea8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workpath"></a><span data-ttu-id="48921-103">Självstudier: Azure Active Directory-integrering med Workpath</span><span class="sxs-lookup"><span data-stu-id="48921-103">Tutorial: Azure Active Directory integration with Workpath</span></span>

<span data-ttu-id="48921-104">I kursen får lära du att integrera Workpath med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="48921-104">In this tutorial, you learn how to integrate Workpath with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48921-105">Integrera Workpath med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="48921-105">Integrating Workpath with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="48921-106">Du kan styra i Azure AD som har åtkomst till Workpath</span><span class="sxs-lookup"><span data-stu-id="48921-106">You can control in Azure AD who has access to Workpath</span></span>
- <span data-ttu-id="48921-107">Du kan aktivera användarna att automatiskt hämta loggat in på Workpath (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="48921-107">You can enable your users to automatically get signed-on to Workpath (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48921-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="48921-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="48921-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48921-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48921-110">Krav</span><span class="sxs-lookup"><span data-stu-id="48921-110">Prerequisites</span></span>

<span data-ttu-id="48921-111">För att konfigurera Azure AD-integrering med Workpath, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="48921-111">To configure Azure AD integration with Workpath, you need the following items:</span></span>

- <span data-ttu-id="48921-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="48921-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48921-113">En Workpath enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="48921-113">A Workpath single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48921-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="48921-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48921-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="48921-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48921-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="48921-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48921-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48921-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48921-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="48921-118">Scenario description</span></span>
<span data-ttu-id="48921-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="48921-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48921-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="48921-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48921-121">Att lägga till Workpath från galleriet</span><span class="sxs-lookup"><span data-stu-id="48921-121">Adding Workpath from the gallery</span></span>
2. <span data-ttu-id="48921-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48921-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workpath-from-the-gallery"></a><span data-ttu-id="48921-123">Att lägga till Workpath från galleriet</span><span class="sxs-lookup"><span data-stu-id="48921-123">Adding Workpath from the gallery</span></span>
<span data-ttu-id="48921-124">Du måste lägga till Workpath från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Workpath i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48921-124">To configure the integration of Workpath into Azure AD, you need to add Workpath from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="48921-125">**Utför följande steg för att lägga till Workpath från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="48921-125">**To add Workpath from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="48921-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="48921-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48921-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="48921-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="48921-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="48921-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="48921-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="48921-133">I sökrutan skriver **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="48921-133">In the search box, type **Workpath**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_search.png)

5. <span data-ttu-id="48921-135">Välj i resultatpanelen **Workpath**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="48921-135">In the results panel, select **Workpath**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48921-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48921-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48921-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workpath baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="48921-138">In this section, you configure and test Azure AD single sign-on with Workpath based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48921-139">Azure AD måste du känna till användaren i Workpath motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="48921-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workpath is to a user in Azure AD.</span></span> <span data-ttu-id="48921-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Workpath upprättas.</span><span class="sxs-lookup"><span data-stu-id="48921-140">In other words, a link relationship between an Azure AD user and the related user in Workpath needs to be established.</span></span>

<span data-ttu-id="48921-141">I Workpath, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="48921-141">In Workpath, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="48921-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Workpath, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="48921-142">To configure and test Azure AD single sign-on with Workpath, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="48921-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="48921-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="48921-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48921-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48921-145">**[Skapa en testanvändare Workpath](#creating-a-workpath-test-user)**  – du har en motsvarighet för Britta Simon i Workpath som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="48921-145">**[Creating a Workpath test user](#creating-a-workpath-test-user)** - to have a counterpart of Britta Simon in Workpath that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="48921-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="48921-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48921-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="48921-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48921-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48921-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48921-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Workpath program.</span><span class="sxs-lookup"><span data-stu-id="48921-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workpath application.</span></span>

<span data-ttu-id="48921-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Workpath:**</span><span class="sxs-lookup"><span data-stu-id="48921-150">**To configure Azure AD single sign-on with Workpath, perform the following steps:**</span></span>

1. <span data-ttu-id="48921-151">I Azure-portalen på den **Workpath** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="48921-151">In the Azure portal, on the **Workpath** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="48921-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="48921-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_samlbase.png)

3. <span data-ttu-id="48921-155">På den **Workpath domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48921-155">On the **Workpath Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url.png)

    <span data-ttu-id="48921-157">a.</span><span class="sxs-lookup"><span data-stu-id="48921-157">a.</span></span> <span data-ttu-id="48921-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://api.workpath.com/v1/saml/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="48921-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.workpath.com/v1/saml/metadata/<instancename>`</span></span>

    <span data-ttu-id="48921-159">b.</span><span class="sxs-lookup"><span data-stu-id="48921-159">b.</span></span> <span data-ttu-id="48921-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://api.workpath.com/v1/saml/assert/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="48921-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.workpath.com/v1/saml/assert/<instancename>`</span></span>

4. <span data-ttu-id="48921-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="48921-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="48921-162">Om du vill konfigurera programmet i **SP** initierade läge, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48921-162">If you wish to configure the application in **SP** initiated mode, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url1.png)

    <span data-ttu-id="48921-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.workpath.com/ `</span><span class="sxs-lookup"><span data-stu-id="48921-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.workpath.com/ `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48921-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="48921-165">These values are not real.</span></span> <span data-ttu-id="48921-166">Uppdatera dessa värden med faktiska inloggnings-URL, identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="48921-166">Update these values with the actual Sign-on URL, Identifier and Reply URL.</span></span> <span data-ttu-id="48921-167">Kontakta [Workpath supportteamet](https://help.workpath.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="48921-167">Contact [Workpath support team](https://help.workpath.com) to get these values.</span></span>

5. <span data-ttu-id="48921-168">Workpath program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="48921-168">Workpath application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="48921-169">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="48921-169">Configure the following claims for this application.</span></span> <span data-ttu-id="48921-170">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="48921-170">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="48921-171">Följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="48921-171">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_attributes.png)
    
6. <span data-ttu-id="48921-173">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48921-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="48921-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="48921-174">Attribute Name</span></span> | <span data-ttu-id="48921-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="48921-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="48921-176">Förnamn</span><span class="sxs-lookup"><span data-stu-id="48921-176">first_name</span></span> | <span data-ttu-id="48921-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="48921-177">user.givenname</span></span> |
    | <span data-ttu-id="48921-178">Efternamn</span><span class="sxs-lookup"><span data-stu-id="48921-178">last_name</span></span> | <span data-ttu-id="48921-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="48921-179">user.surname</span></span> |
    
    <span data-ttu-id="48921-180">a.</span><span class="sxs-lookup"><span data-stu-id="48921-180">a.</span></span> <span data-ttu-id="48921-181">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="48921-183">b.</span><span class="sxs-lookup"><span data-stu-id="48921-183">b.</span></span> <span data-ttu-id="48921-184">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="48921-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="48921-186">c.</span><span class="sxs-lookup"><span data-stu-id="48921-186">c.</span></span> <span data-ttu-id="48921-187">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="48921-187">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="48921-188">d.</span><span class="sxs-lookup"><span data-stu-id="48921-188">d.</span></span> <span data-ttu-id="48921-189">Lämna den **Namespace** textruta tomt.</span><span class="sxs-lookup"><span data-stu-id="48921-189">Leave the **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="48921-190">e.</span><span class="sxs-lookup"><span data-stu-id="48921-190">e.</span></span> <span data-ttu-id="48921-191">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="48921-191">Click **Ok**.</span></span>
    

7. <span data-ttu-id="48921-192">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="48921-192">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_certificate.png) 

8. <span data-ttu-id="48921-194">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="48921-194">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="48921-196">På den **Workpath Configuration** klickar du på **konfigurera Workpath** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="48921-196">On the **Workpath Configuration** section, click **Configure Workpath** to open **Configure sign-on** window.</span></span> <span data-ttu-id="48921-197">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="48921-197">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_configure.png) 

10. <span data-ttu-id="48921-199">Konfigurera enkel inloggning på **Workpath** sida, måste du skicka den hämtade **XML-Metadata för**, **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Workpath supportteamet](https://help.workpath.com).</span><span class="sxs-lookup"><span data-stu-id="48921-199">To configure single sign-on on **Workpath** side, you need to send the downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Workpath support team](https://help.workpath.com).</span></span> 

> [!TIP]
> <span data-ttu-id="48921-200">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="48921-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="48921-201">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="48921-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="48921-202">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48921-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48921-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="48921-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="48921-204">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48921-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="48921-206">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="48921-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="48921-207">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="48921-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48921-209">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="48921-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48921-211">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48921-213">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48921-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48921-215">a.</span><span class="sxs-lookup"><span data-stu-id="48921-215">a.</span></span> <span data-ttu-id="48921-216">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48921-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48921-217">b.</span><span class="sxs-lookup"><span data-stu-id="48921-217">b.</span></span> <span data-ttu-id="48921-218">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48921-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48921-219">c.</span><span class="sxs-lookup"><span data-stu-id="48921-219">c.</span></span> <span data-ttu-id="48921-220">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="48921-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="48921-221">d.</span><span class="sxs-lookup"><span data-stu-id="48921-221">d.</span></span> <span data-ttu-id="48921-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="48921-222">Click **Create**.</span></span>
 
### <a name="creating-a-workpath-test-user"></a><span data-ttu-id="48921-223">Skapa en testanvändare Workpath</span><span class="sxs-lookup"><span data-stu-id="48921-223">Creating a Workpath test user</span></span>

<span data-ttu-id="48921-224">Workpath stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="48921-224">Workpath supports Just in time user provisioning.</span></span> <span data-ttu-id="48921-225">Efter autentisering skapas användare i programmet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="48921-225">After authentication users are created in the application automatically.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="48921-226">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="48921-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="48921-227">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Workpath.</span><span class="sxs-lookup"><span data-stu-id="48921-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workpath.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="48921-229">**Om du vill tilldela Workpath Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="48921-229">**To assign Britta Simon to Workpath, perform the following steps:**</span></span>

1. <span data-ttu-id="48921-230">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="48921-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="48921-232">Välj i listan med program **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="48921-232">In the applications list, select **Workpath**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_app.png) 

3. <span data-ttu-id="48921-234">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="48921-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="48921-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="48921-236">Click **Add** button.</span></span> <span data-ttu-id="48921-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="48921-239">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="48921-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="48921-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48921-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48921-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48921-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48921-242">Testing single sign-on</span></span>

<span data-ttu-id="48921-243">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="48921-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="48921-244">När du klickar på panelen Workpath på åtkomstpanelen du bör få automatiskt loggat in på ditt Workpath program.</span><span class="sxs-lookup"><span data-stu-id="48921-244">When you click the Workpath tile in the Access Panel, you should get automatically signed-on to your Workpath application.</span></span>
<span data-ttu-id="48921-245">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="48921-245">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48921-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="48921-246">Additional resources</span></span>

* [<span data-ttu-id="48921-247">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48921-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48921-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48921-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_203.png

