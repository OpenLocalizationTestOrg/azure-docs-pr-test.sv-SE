---
title: "Självstudier: Azure Active Directory-integrering med Tableau Online | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Tableau Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="c3fb6-103">Självstudier: Azure Active Directory-integrering med Tableau Online</span><span class="sxs-lookup"><span data-stu-id="c3fb6-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="c3fb6-104">Lär dig hur du integrerar Tableau Online med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-104">In this tutorial, you learn how to integrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c3fb6-105">Integrera Tableau Online med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-105">Integrating Tableau Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c3fb6-106">Du kan styra i Azure AD som har åtkomst till Tableau Online</span><span class="sxs-lookup"><span data-stu-id="c3fb6-106">You can control in Azure AD who has access to Tableau Online</span></span>
- <span data-ttu-id="c3fb6-107">Du kan aktivera användarna att automatiskt hämta loggat in på Tableau Online (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c3fb6-107">You can enable your users to automatically get signed-on to Tableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c3fb6-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c3fb6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c3fb6-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3fb6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c3fb6-110">Prerequisites</span></span>

<span data-ttu-id="c3fb6-111">För att konfigurera Azure AD-integrering med Tableau Online, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-111">To configure Azure AD integration with Tableau Online, you need the following items:</span></span>

- <span data-ttu-id="c3fb6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c3fb6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c3fb6-113">En Tableau Online enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c3fb6-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c3fb6-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c3fb6-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c3fb6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c3fb6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c3fb6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c3fb6-118">Scenario description</span></span>
<span data-ttu-id="c3fb6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c3fb6-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c3fb6-121">Att lägga till Tableau Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="c3fb6-121">Adding Tableau Online from the gallery</span></span>
2. <span data-ttu-id="c3fb6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c3fb6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-the-gallery"></a><span data-ttu-id="c3fb6-123">Att lägga till Tableau Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="c3fb6-123">Adding Tableau Online from the gallery</span></span>
<span data-ttu-id="c3fb6-124">Du måste lägga till Tableau Online från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Tableau Online i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-124">To configure the integration of Tableau Online into Azure AD, you need to add Tableau Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c3fb6-125">**Om du vill lägga till Tableau Online från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-125">**To add Tableau Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c3fb6-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c3fb6-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c3fb6-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c3fb6-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c3fb6-133">I sökrutan skriver **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-133">In the search box, type **Tableau Online**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="c3fb6-135">Välj i resultatpanelen **Tableau Online**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-135">In the results panel, select **Tableau Online**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c3fb6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c3fb6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c3fb6-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tableau Online baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c3fb6-139">Azure AD måste du känna till motsvarande användaren i Tableau Online till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Online is to a user in Azure AD.</span></span> <span data-ttu-id="c3fb6-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Tableau Online upprättas.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Online needs to be established.</span></span>

<span data-ttu-id="c3fb6-141">I Tableau Online, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-141">In Tableau Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c3fb6-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Tableau Online, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-142">To configure and test Azure AD single sign-on with Tableau Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c3fb6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c3fb6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c3fb6-145">**[Skapa en Tableau Online testanvändare](#creating-a-tableau-online-test-user)**  – du har en motsvarighet för Britta Simon i Tableau Online som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - to have a counterpart of Britta Simon in Tableau Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c3fb6-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c3fb6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c3fb6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c3fb6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c3fb6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="c3fb6-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Tableau Online:**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-150">**To configure Azure AD single sign-on with Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="c3fb6-151">I Azure-portalen på den **Tableau Online** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-151">In the Azure portal, on the **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c3fb6-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="c3fb6-155">På den **Tableau Online domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-155">On the **Tableau Online Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="c3fb6-157">a.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-157">a.</span></span> <span data-ttu-id="c3fb6-158">I den **inloggnings-URL** textruta anger du URL:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="c3fb6-158">In the **Sign-on URL** textbox, type the URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="c3fb6-159">b.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-159">b.</span></span> <span data-ttu-id="c3fb6-160">I den **identifierare** textruta anger du URL:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="c3fb6-160">In the **Identifier** textbox, type the URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="c3fb6-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="c3fb6-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c3fb6-165">I ett annat webbläsarfönster inloggning i tillämpningsprogrammet Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-165">In a different browser window, sign-on to your Tableau Online application.</span></span> <span data-ttu-id="c3fb6-166">Gå till **inställningar** och sedan **autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-166">Go to **Settings** and then **Authentication**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="c3fb6-168">Aktivera SAML Under **autentiseringstyper** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-168">To enable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="c3fb6-169">Kontrollera den **enkel inloggning med SAML** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-169">Check the **Single sign-on with SAML** checkbox.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="c3fb6-171">Rulla nedåt tills **metadata importfilen till Tableau Online** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="c3fb6-172">Klicka på Bläddra och importera metadatafilen som du har hämtat från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-172">Click Browse and import the metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="c3fb6-173">Klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-173">Then, click **Apply**.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="c3fb6-175">I den **matchar intyg** avsnittet, infoga motsvarande identitetsleverantör assertion namn för **e-postadress**, **Förnamn**, och **efternamn**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-175">In the **Match assertions** section, insert the corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="c3fb6-176">Hämta informationen från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-176">To get this information from Azure AD:</span></span> 
  
    <span data-ttu-id="c3fb6-177">a.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-177">a.</span></span> <span data-ttu-id="c3fb6-178">I Azure-portalen går du den **Tableau Online** sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-178">In the Azure portal, go on the **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="c3fb6-179">b.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-179">b.</span></span> <span data-ttu-id="c3fb6-180">I attributavsnittet väljer den **”visa och redigera andra användarattribut”** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-180">In the attributes section, Select the **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="c3fb6-182">c.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-182">c.</span></span> <span data-ttu-id="c3fb6-183">Kopiera namnområdesvärdet för dessa attribut: givenname, e-post- och efternamn med hjälp av följande steg:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-183">Copy the namespace value for these attributes: givenname, email and surname by using the following steps:</span></span>

   ![Azure AD-Single Sign-On](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="c3fb6-185">d.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-185">d.</span></span> <span data-ttu-id="c3fb6-186">Klicka på **user.givenname** värde</span><span class="sxs-lookup"><span data-stu-id="c3fb6-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="c3fb6-187">e.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-187">e.</span></span> <span data-ttu-id="c3fb6-188">Kopiera värdet från den **namnområde** textruta.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-188">Copy the value from the **namespace** textbox.</span></span>

   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="c3fb6-190">f.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-190">f.</span></span> <span data-ttu-id="c3fb6-191">Om du vill kopiera namesapce anvisningarna värden för efternamn och e-ovan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-191">To copy the namesapce values for the email and surname follow the preceding steps.</span></span>

    <span data-ttu-id="c3fb6-192">g.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-192">g.</span></span> <span data-ttu-id="c3fb6-193">Växla till programmet Tableau Online, och sedan ange den **Tableau Onlineattribut** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-193">Switch to the Tableau Online application, then set the **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="c3fb6-194">E-post: **e** eller **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="c3fb6-195">Förnamn: **givenname**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-195">First name: **givenname**</span></span>
     * <span data-ttu-id="c3fb6-196">Efternamn: **efternamn**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-196">Last name: **surname**</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="c3fb6-198">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c3fb6-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c3fb6-199">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c3fb6-200">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c3fb6-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c3fb6-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c3fb6-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="c3fb6-202">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c3fb6-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c3fb6-205">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c3fb6-207">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c3fb6-209">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c3fb6-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c3fb6-213">a.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-213">a.</span></span> <span data-ttu-id="c3fb6-214">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c3fb6-215">b.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-215">b.</span></span> <span data-ttu-id="c3fb6-216">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c3fb6-217">c.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-217">c.</span></span> <span data-ttu-id="c3fb6-218">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c3fb6-219">d.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-219">d.</span></span> <span data-ttu-id="c3fb6-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="c3fb6-221">Skapa en Tableau Online testanvändare</span><span class="sxs-lookup"><span data-stu-id="c3fb6-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="c3fb6-222">I det här avsnittet skapar du en användare som kallas Britta Simon i Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="c3fb6-223">På **Tableau Online**, klickar du på **inställningar** och sedan **autentisering** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="c3fb6-224">Rulla ned till **Välj användare** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-224">Scroll down to **Select Users** section.</span></span> <span data-ttu-id="c3fb6-225">Klicka på **lägga till användare** och sedan **ange e-postadresser**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="c3fb6-227">Välj **lägga till användare för enkel inloggning (SSO) autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="c3fb6-228">I den **ange e-postadresser** textruta Lägg tillbritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="c3fb6-228">In the **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="c3fb6-230">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-230">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c3fb6-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c3fb6-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c3fb6-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Online.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c3fb6-234">**Om du vill tilldela Britta Simon Tableau Online, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c3fb6-234">**To assign Britta Simon to Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="c3fb6-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c3fb6-237">Välj i listan med program **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-237">In the applications list, select **Tableau Online**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="c3fb6-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c3fb6-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-241">Click **Add** button.</span></span> <span data-ttu-id="c3fb6-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c3fb6-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c3fb6-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c3fb6-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c3fb6-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c3fb6-247">Testing single sign-on</span></span>

<span data-ttu-id="c3fb6-248">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="c3fb6-249">När du klickar på panelen Tableau Online på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-249">When you click the Tableau Online tile in the Access Panel, you should get automatically signed-on to your Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3fb6-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c3fb6-250">Additional resources</span></span>

* [<span data-ttu-id="c3fb6-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c3fb6-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c3fb6-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c3fb6-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

