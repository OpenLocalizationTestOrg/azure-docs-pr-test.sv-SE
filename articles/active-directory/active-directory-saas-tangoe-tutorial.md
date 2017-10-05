---
title: "Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Tangoe kommandot Premium Mobile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 595541e7248a7486e58123927389c552af0e50f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a><span data-ttu-id="33abe-103">Självstudier: Azure Active Directory-integrering med Tangoe kommandot Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="33abe-103">Tutorial: Azure Active Directory integration with Tangoe Command Premium Mobile</span></span>

<span data-ttu-id="33abe-104">I kursen får lära du att integrera Tangoe kommandot Premium Mobile med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="33abe-104">In this tutorial, you learn how to integrate Tangoe Command Premium Mobile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="33abe-105">Integrera Tangoe kommandot Premium Mobile med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="33abe-105">Integrating Tangoe Command Premium Mobile with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="33abe-106">Du kan styra i Azure AD som har åtkomst till Tangoe kommandot Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="33abe-106">You can control in Azure AD who has access to Tangoe Command Premium Mobile</span></span>
- <span data-ttu-id="33abe-107">Du kan aktivera användarna att automatiskt hämta loggat in på Tangoe kommandot Premium Mobile (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="33abe-107">You can enable your users to automatically get signed-on to Tangoe Command Premium Mobile (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="33abe-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33abe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="33abe-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="33abe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33abe-110">Krav</span><span class="sxs-lookup"><span data-stu-id="33abe-110">Prerequisites</span></span>

<span data-ttu-id="33abe-111">Om du vill konfigurera Azure AD-integrering med Tangoe kommandot Premium Mobile behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="33abe-111">To configure Azure AD integration with Tangoe Command Premium Mobile, you need the following items:</span></span>

- <span data-ttu-id="33abe-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="33abe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="33abe-113">En Tangoe kommandot Premium Mobile enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="33abe-113">A Tangoe Command Premium Mobile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="33abe-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="33abe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="33abe-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="33abe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="33abe-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="33abe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="33abe-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="33abe-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="33abe-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="33abe-118">Scenario description</span></span>
<span data-ttu-id="33abe-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="33abe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="33abe-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="33abe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="33abe-121">Lägg till Tangoe kommandot Premium Mobile från galleriet</span><span class="sxs-lookup"><span data-stu-id="33abe-121">Add Tangoe Command Premium Mobile from the gallery</span></span>
2. <span data-ttu-id="33abe-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33abe-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a><span data-ttu-id="33abe-123">Lägg till Tangoe kommandot Premium Mobile från galleriet</span><span class="sxs-lookup"><span data-stu-id="33abe-123">Add Tangoe Command Premium Mobile from the gallery</span></span>
<span data-ttu-id="33abe-124">Du måste lägga till Tangoe kommandot Premium Mobile från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Tangoe kommandot Premium Mobile i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="33abe-124">To configure the integration of Tangoe Command Premium Mobile into Azure AD, you need to add Tangoe Command Premium Mobile from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="33abe-125">**Utför följande steg för att lägga till Tangoe kommandot Premium Mobile från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="33abe-125">**To add Tangoe Command Premium Mobile from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="33abe-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33abe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="33abe-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="33abe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="33abe-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="33abe-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="33abe-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33abe-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="33abe-133">I sökrutan skriver **Tangoe kommandot Premium Mobile**väljer **Tangoe kommandot Premium Mobile** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="33abe-133">In the search box, type **Tangoe Command Premium Mobile**, select **Tangoe Command Premium Mobile** from result panel then click **Add** button to add the application.</span></span>

    ![<span data-ttu-id="33abe-134">Lägg till Tangoe kommandot Premium Mobile från galleriet</span><span class="sxs-lookup"><span data-stu-id="33abe-134">Add Tangoe Command Premium Mobile from gallery</span></span> ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="33abe-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33abe-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="33abe-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="33abe-136">In this section, you configure and test Azure AD single sign-on with Tangoe Command Premium Mobile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="33abe-137">Azure AD måste du känna till motsvarande användaren i Tangoe kommandot Premium Mobile till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="33abe-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Tangoe Command Premium Mobile is to a user in Azure AD.</span></span> <span data-ttu-id="33abe-138">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Tangoe kommandot Premium Mobile upprättas.</span><span class="sxs-lookup"><span data-stu-id="33abe-138">In other words, a link relationship between an Azure AD user and the related user in Tangoe Command Premium Mobile needs to be established.</span></span>

<span data-ttu-id="33abe-139">I Tangoe kommandot Premium Mobile tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="33abe-139">In Tangoe Command Premium Mobile, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="33abe-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Tangoe kommandot Premium Mobile, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="33abe-140">To configure and test Azure AD single sign-on with Tangoe Command Premium Mobile, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="33abe-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="33abe-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="33abe-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33abe-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="33abe-143">**[Skapa en testanvändare Tangoe kommandot Premium Mobile](#create-a-tangoe-command-premium-mobile-test-user)**  – du har en motsvarighet för Britta Simon i Tangoe kommandot Premium Mobile som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="33abe-143">**[Create a Tangoe Command Premium Mobile test user](#create-a-tangoe-command-premium-mobile-test-user)** - to have a counterpart of Britta Simon in Tangoe Command Premium Mobile that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="33abe-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33abe-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="33abe-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="33abe-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="33abe-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33abe-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="33abe-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Tangoe kommandot Premium mobila program.</span><span class="sxs-lookup"><span data-stu-id="33abe-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tangoe Command Premium Mobile application.</span></span>

<span data-ttu-id="33abe-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Tangoe kommandot Premium Mobile:**</span><span class="sxs-lookup"><span data-stu-id="33abe-148">**To configure Azure AD single sign-on with Tangoe Command Premium Mobile, perform the following steps:**</span></span>

1. <span data-ttu-id="33abe-149">I Azure-portalen på den **Tangoe kommandot Premium Mobile** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="33abe-149">In the Azure portal, on the **Tangoe Command Premium Mobile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="33abe-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33abe-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. <span data-ttu-id="33abe-153">På den **Tangoe kommandot Premium Mobile domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="33abe-153">On the **Tangoe Command Premium Mobile Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Tangoe kommandot Premium mobila domän](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    <span data-ttu-id="33abe-155">a.</span><span class="sxs-lookup"><span data-stu-id="33abe-155">a.</span></span> <span data-ttu-id="33abe-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span><span class="sxs-lookup"><span data-stu-id="33abe-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span></span>

    <span data-ttu-id="33abe-157">b.</span><span class="sxs-lookup"><span data-stu-id="33abe-157">b.</span></span> <span data-ttu-id="33abe-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://sso.tangoe.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="33abe-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://sso.tangoe.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="33abe-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="33abe-159">These values are not real.</span></span> <span data-ttu-id="33abe-160">Uppdatera dessa värden med den faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="33abe-160">Update these values with the actual  Reply URL and Sign-On URL.</span></span> <span data-ttu-id="33abe-161">Kontakta [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="33abe-161">Contact [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) to get these values.</span></span> 

4. <span data-ttu-id="33abe-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="33abe-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. <span data-ttu-id="33abe-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="33abe-164">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="33abe-166">På den **Tangoe Premium Mobile konfiguration** klickar du på **konfigurera Tangoe kommandot Premium Mobile** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="33abe-166">On the **Tangoe Command Premium Mobile Configuration** section, click **Configure Tangoe Command Premium Mobile** to open **Configure sign-on** window.</span></span> <span data-ttu-id="33abe-167">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="33abe-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurationsavsnittet Tangoe kommandot Premium Mobile](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. <span data-ttu-id="33abe-169">För att få SSO konfigurerats för ditt program, kontakta din [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) och ange följande:</span><span class="sxs-lookup"><span data-stu-id="33abe-169">To get SSO configured for your application, contact your [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) and provide the following:</span></span>

   - <span data-ttu-id="33abe-170">Den hämtade metadatafilen</span><span class="sxs-lookup"><span data-stu-id="33abe-170">The downloaded metadata file</span></span>
   - <span data-ttu-id="33abe-171">Den **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="33abe-171">The **SAML Entity ID**</span></span>
   - <span data-ttu-id="33abe-172">Den **URL för SAML-tjänst för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="33abe-172">The **SAML Single Sign-On Service URL**</span></span>
   - <span data-ttu-id="33abe-173">Den **URL för utloggning**</span><span class="sxs-lookup"><span data-stu-id="33abe-173">The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="33abe-174">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="33abe-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="33abe-175">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="33abe-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="33abe-176">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="33abe-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="33abe-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="33abe-177">Create an Azure AD test user</span></span>
<span data-ttu-id="33abe-178">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33abe-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="33abe-180">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="33abe-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="33abe-181">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33abe-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="33abe-183">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="33abe-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="33abe-185">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33abe-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Lägga till användare](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="33abe-187">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="33abe-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="33abe-189">a.</span><span class="sxs-lookup"><span data-stu-id="33abe-189">a.</span></span> <span data-ttu-id="33abe-190">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="33abe-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="33abe-191">b.</span><span class="sxs-lookup"><span data-stu-id="33abe-191">b.</span></span> <span data-ttu-id="33abe-192">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="33abe-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="33abe-193">c.</span><span class="sxs-lookup"><span data-stu-id="33abe-193">c.</span></span> <span data-ttu-id="33abe-194">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="33abe-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="33abe-195">d.</span><span class="sxs-lookup"><span data-stu-id="33abe-195">d.</span></span> <span data-ttu-id="33abe-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="33abe-196">Click **Create**.</span></span>
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a><span data-ttu-id="33abe-197">Skapa en Tangoe kommandot Premium mobila användare</span><span class="sxs-lookup"><span data-stu-id="33abe-197">Create a Tangoe Command Premium Mobile test user</span></span>

<span data-ttu-id="33abe-198">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Tangoe kommandot Premium Mobile.</span><span class="sxs-lookup"><span data-stu-id="33abe-198">In this section, you create a user called Britta Simon in Tangoe Command Premium Mobile.</span></span> 

<span data-ttu-id="33abe-199">Tangoe kommandot Premium Mobile-programmet måste alla användare som ska etableras i programmet innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33abe-199">Tangoe Command Premium Mobile application needs all the users to be provisioned in the application before doing Single Sign On.</span></span> <span data-ttu-id="33abe-200">Så kan du arbeta med den [Tangoe kommandot Premium Mobile Client supportteamet](https://www.tangoe.com/contact-2/) att etablera dessa användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="33abe-200">So please work with the [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) to provision all these users into the application.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="33abe-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="33abe-201">Assign the Azure AD test user</span></span>

<span data-ttu-id="33abe-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Tangoe kommandot Premium Mobile.</span><span class="sxs-lookup"><span data-stu-id="33abe-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tangoe Command Premium Mobile.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="33abe-204">**Om du vill tilldela Tangoe kommandot Premium Mobile Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33abe-204">**To assign Britta Simon to Tangoe Command Premium Mobile, perform the following steps:**</span></span>

1. <span data-ttu-id="33abe-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="33abe-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="33abe-207">Välj i listan med program **Tangoe kommandot Premium Mobile**.</span><span class="sxs-lookup"><span data-stu-id="33abe-207">In the applications list, select **Tangoe Command Premium Mobile**.</span></span>

    ![Tangoe kommandot Premium Mobile i listan över appar](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. <span data-ttu-id="33abe-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="33abe-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="33abe-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="33abe-211">Click **Add** button.</span></span> <span data-ttu-id="33abe-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33abe-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="33abe-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="33abe-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="33abe-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33abe-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="33abe-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33abe-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="33abe-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33abe-217">Test single sign-on</span></span>

<span data-ttu-id="33abe-218">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="33abe-218">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="33abe-219">När du klickar på panelen Tangoe kommandot Premium Mobile på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Tangoe kommandot Premium mobila program.</span><span class="sxs-lookup"><span data-stu-id="33abe-219">When you click the Tangoe Command Premium Mobile tile in the Access Panel, you should get automatically signed-on to your Tangoe Command Premium Mobile application.</span></span> <span data-ttu-id="33abe-220">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="33abe-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="33abe-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="33abe-221">Additional resources</span></span>

* [<span data-ttu-id="33abe-222">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="33abe-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="33abe-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="33abe-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

