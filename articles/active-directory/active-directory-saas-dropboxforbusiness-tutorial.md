---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a><span data-ttu-id="dec8a-103">Självstudier: Azure Active Directory-integrering med Dropbox för företag</span><span class="sxs-lookup"><span data-stu-id="dec8a-103">Tutorial: Azure Active Directory integration with Dropbox for Business</span></span>

<span data-ttu-id="dec8a-104">I kursen får lära du att integrera Dropbox för företag med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dec8a-104">In this tutorial, you learn how to integrate Dropbox for Business with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dec8a-105">Integrera Dropbox för företag med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dec8a-105">Integrating Dropbox for Business with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dec8a-106">Du kan styra i Azure AD som har åtkomst till Dropbox för företag</span><span class="sxs-lookup"><span data-stu-id="dec8a-106">You can control in Azure AD who has access to Dropbox for Business</span></span>
- <span data-ttu-id="dec8a-107">Du kan aktivera användarna att automatiskt hämta loggat in på Dropbox för företag (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="dec8a-107">You can enable your users to automatically get signed-on to Dropbox for Business (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dec8a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dec8a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dec8a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dec8a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dec8a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dec8a-110">Prerequisites</span></span>

<span data-ttu-id="dec8a-111">För att konfigurera Azure AD-integrering med Dropbox för företag, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="dec8a-111">To configure Azure AD integration with Dropbox for Business, you need the following items:</span></span>

- <span data-ttu-id="dec8a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dec8a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dec8a-113">En Dropbox för Business enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="dec8a-113">A Dropbox for Business single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dec8a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dec8a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dec8a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dec8a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dec8a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dec8a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dec8a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dec8a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dec8a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dec8a-118">Scenario description</span></span>
<span data-ttu-id="dec8a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dec8a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dec8a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dec8a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dec8a-121">Att lägga till Dropbox för företag från galleriet</span><span class="sxs-lookup"><span data-stu-id="dec8a-121">Adding Dropbox for Business from the gallery</span></span>
2. <span data-ttu-id="dec8a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dec8a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dropbox-for-business-from-the-gallery"></a><span data-ttu-id="dec8a-123">Att lägga till Dropbox för företag från galleriet</span><span class="sxs-lookup"><span data-stu-id="dec8a-123">Adding Dropbox for Business from the gallery</span></span>
<span data-ttu-id="dec8a-124">Du måste lägga till Dropbox för företag från galleriet i listan över hanterade SaaS-appar för att konfigurera Dropbox för företag till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="dec8a-124">To configure the integration of Dropbox for Business into Azure AD, you need to add Dropbox for Business from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dec8a-125">**Utför följande steg för att lägga till Dropbox för företag från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="dec8a-125">**To add Dropbox for Business from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dec8a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dec8a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dec8a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dec8a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="dec8a-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dec8a-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="dec8a-133">I sökrutan skriver **Dropbox för företag**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-133">In the search box, type **Dropbox for Business**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. <span data-ttu-id="dec8a-135">Välj i resultatpanelen **Dropbox för företag**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="dec8a-135">In the results panel, select **Dropbox for Business**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dec8a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dec8a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dec8a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Dropbox för företag som baseras på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dec8a-138">In this section, you configure and test Azure AD single sign-on with Dropbox for Business based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dec8a-139">Azure AD måste du känna till motsvarande användaren i Dropbox för företag till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="dec8a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Dropbox for Business is to a user in Azure AD.</span></span> <span data-ttu-id="dec8a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Dropbox för företag upprättas.</span><span class="sxs-lookup"><span data-stu-id="dec8a-140">In other words, a link relationship between an Azure AD user and the related user in Dropbox for Business needs to be established.</span></span>

<span data-ttu-id="dec8a-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="dec8a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Dropbox for Business.</span></span>

<span data-ttu-id="dec8a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Dropbox för företag, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="dec8a-142">To configure and test Azure AD single sign-on with Dropbox for Business, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dec8a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="dec8a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dec8a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dec8a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dec8a-145">**[Skapa en Dropbox för Business testanvändare](#creating-a-dropbox-for-business-test-user)**  – du har en motsvarighet för Britta Simon i Dropbox för företag som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="dec8a-145">**[Creating a Dropbox for Business test user](#creating-a-dropbox-for-business-test-user)** - to have a counterpart of Britta Simon in Dropbox for Business that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dec8a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dec8a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dec8a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="dec8a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dec8a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dec8a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dec8a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Dropbox för affärsprogram.</span><span class="sxs-lookup"><span data-stu-id="dec8a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dropbox for Business application.</span></span>

<span data-ttu-id="dec8a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Dropbox för företag:**</span><span class="sxs-lookup"><span data-stu-id="dec8a-150">**To configure Azure AD single sign-on with Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="dec8a-151">I Azure-portalen på den **Dropbox för företag** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-151">In the Azure portal, on the **Dropbox for Business** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="dec8a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dec8a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. <span data-ttu-id="dec8a-155">På den **Dropbox Business domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dec8a-155">On the **Dropbox for Business Domain and URLs** section, perform the following steps:</span></span>

    <span data-ttu-id="dec8a-156">a.</span><span class="sxs-lookup"><span data-stu-id="dec8a-156">a.</span></span> <span data-ttu-id="dec8a-157">Logga in på din Dropbox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="dec8a-157">Sign on to your Dropbox for business tenant.</span></span> 
   
    <span data-ttu-id="dec8a-158">![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dec8a-158">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dec8a-159">b.</span><span class="sxs-lookup"><span data-stu-id="dec8a-159">b.</span></span> <span data-ttu-id="dec8a-160">I navigeringsfönstret till vänster klickar du på **administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-160">In the navigation pane on the left side, click **Admin Console**.</span></span> 
   
    <span data-ttu-id="dec8a-161">![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dec8a-161">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dec8a-162">c.</span><span class="sxs-lookup"><span data-stu-id="dec8a-162">c.</span></span> <span data-ttu-id="dec8a-163">På den **administratörskonsolen**, klickar du på **autentisering** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="dec8a-163">On the **Admin Console**, click **Authentication** in the left navigation pane.</span></span> 
   
    <span data-ttu-id="dec8a-164">![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dec8a-164">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dec8a-165">d.</span><span class="sxs-lookup"><span data-stu-id="dec8a-165">d.</span></span> <span data-ttu-id="dec8a-166">I den **enkel inloggning** väljer **aktivera enkel inloggning**, och klicka sedan på **mer** att expandera det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dec8a-166">In the **Single sign-on** section, select **Enable single sign-on**, and then click **More** to expand this section.</span></span>  
   
    <span data-ttu-id="dec8a-167">![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dec8a-167">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dec8a-168">e.</span><span class="sxs-lookup"><span data-stu-id="dec8a-168">e.</span></span> <span data-ttu-id="dec8a-169">Kopiera URL-Adressen bredvid **användarna kan logga in genom att ange sina e-postadress eller de kan gå direkt till**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-169">Copy the URL next to **Users can sign in by entering their email address or they can go directly to**.</span></span> 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    <span data-ttu-id="dec8a-171">f.</span><span class="sxs-lookup"><span data-stu-id="dec8a-171">f.</span></span> <span data-ttu-id="dec8a-172">På Azure-portalen i den **inloggnings-URL** textruta klistra in Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="dec8a-172">On the Azure portal, in the **Sign-on URL** textbox, paste the URL.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     <span data-ttu-id="dec8a-174">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.dropbox.com/sso/<id>`</span><span class="sxs-lookup"><span data-stu-id="dec8a-174">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.dropbox.com/sso/<id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dec8a-175">Det här värdet är inte verkliga värde.</span><span class="sxs-lookup"><span data-stu-id="dec8a-175">This value is not real value.</span></span> <span data-ttu-id="dec8a-176">Uppdatera värdet med det faktiska inloggnings-URL du får från deras avsnitt för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dec8a-176">Update the value with the actual Sign-on URL you get from their Single sign-on section.</span></span> <span data-ttu-id="dec8a-177">Kontakta [Dropbox för Business Client supportteamet](https://www.dropbox.com/business/contact) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="dec8a-177">Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact) to get this value.</span></span> 
 
4. <span data-ttu-id="dec8a-178">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="dec8a-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. <span data-ttu-id="dec8a-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="dec8a-180">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dec8a-182">På den **Dropbox för konfiguration av företag** klickar du på **konfigurera Dropbox för företag** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="dec8a-182">On the **Dropbox for Business Configuration** section, click **Configure Dropbox for Business** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dec8a-183">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="dec8a-183">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. <span data-ttu-id="dec8a-185">Konfigurera enkel inloggning på **Dropbox för företag** sidan finns på din Dropbox för företag-klient i den **enkel inloggning** avsnitt i den **autentisering** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dec8a-185">To configure single sign-on on **Dropbox for Business** side, Go on your Dropbox for Business tenant, in the **Single sign-on** section of the **Authentication** page, perform the following steps:</span></span> 
   
    <span data-ttu-id="dec8a-186">![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dec8a-186">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="dec8a-187">a.</span><span class="sxs-lookup"><span data-stu-id="dec8a-187">a.</span></span> <span data-ttu-id="dec8a-188">Klicka på **krävs**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-188">Click **Required**.</span></span>
   
    <span data-ttu-id="dec8a-189">b.</span><span class="sxs-lookup"><span data-stu-id="dec8a-189">b.</span></span> <span data-ttu-id="dec8a-190">I Azure-portalen på den **konfigurera inloggning** fönster, kopiera den **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in det i den **inloggning URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="dec8a-190">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in URL** textbox.</span></span>

    <span data-ttu-id="dec8a-191">c.</span><span class="sxs-lookup"><span data-stu-id="dec8a-191">c.</span></span> <span data-ttu-id="dec8a-192">Klicka på **Välj certifikat**, och bläddra sedan till din **Base64-kodade certifikatfilen**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-192">Click **Choose certificate**, and then browse to your **Base64 encoded certificate file**.</span></span>

    <span data-ttu-id="dec8a-193">d.</span><span class="sxs-lookup"><span data-stu-id="dec8a-193">d.</span></span> <span data-ttu-id="dec8a-194">Klicka på **spara ändringar** för att slutföra konfigurationen på din DropBox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="dec8a-194">Click **Save changes** to complete the configuration on your DropBox for Business tenant.</span></span>

> [!TIP]
> <span data-ttu-id="dec8a-195">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="dec8a-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dec8a-196">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="dec8a-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dec8a-197">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dec8a-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dec8a-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dec8a-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="dec8a-199">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dec8a-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="dec8a-201">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="dec8a-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dec8a-202">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dec8a-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="dec8a-204">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dec8a-206">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dec8a-206">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dec8a-208">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dec8a-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dec8a-210">a.</span><span class="sxs-lookup"><span data-stu-id="dec8a-210">a.</span></span> <span data-ttu-id="dec8a-211">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dec8a-212">b.</span><span class="sxs-lookup"><span data-stu-id="dec8a-212">b.</span></span> <span data-ttu-id="dec8a-213">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dec8a-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dec8a-214">c.</span><span class="sxs-lookup"><span data-stu-id="dec8a-214">c.</span></span> <span data-ttu-id="dec8a-215">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dec8a-216">d.</span><span class="sxs-lookup"><span data-stu-id="dec8a-216">d.</span></span> <span data-ttu-id="dec8a-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-217">Click **Create**.</span></span>
 
### <a name="creating-a-dropbox-for-business-test-user"></a><span data-ttu-id="dec8a-218">Skapa en Dropbox för Business testanvändare</span><span class="sxs-lookup"><span data-stu-id="dec8a-218">Creating a Dropbox for Business test user</span></span>

<span data-ttu-id="dec8a-219">I det här avsnittet skapas en användare som kallas Britta Simon i Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="dec8a-219">In this section, a user called Britta Simon is created in Dropbox for Business.</span></span> <span data-ttu-id="dec8a-220">Dropbox för företag stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="dec8a-220">Dropbox for Business supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="dec8a-221">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dec8a-221">There is no action item for you in this section.</span></span> <span data-ttu-id="dec8a-222">Om en användare inte redan finns i Dropbox för företag, skapas en ny när du försöker få åtkomst till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="dec8a-222">If a user doesn't already exist in Dropbox for Business, a new one is created when you attempt to access Dropbox for Business.</span></span>

>[!Note]
><span data-ttu-id="dec8a-223">Om du behöver skapa en användare manuellt, kontakta [Dropbox för Business Client supportteamet](https://www.dropbox.com/business/contact)</span><span class="sxs-lookup"><span data-stu-id="dec8a-223">If you need to create a user manually, Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact)</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dec8a-224">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="dec8a-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dec8a-225">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="dec8a-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dropbox for Business.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="dec8a-227">**Utför följande steg om du vill tilldela Britta Simon till Dropbox för företag:**</span><span class="sxs-lookup"><span data-stu-id="dec8a-227">**To assign Britta Simon to Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="dec8a-228">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="dec8a-230">Välj i listan med program **Dropbox för företag**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-230">In the applications list, select **Dropbox for Business**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. <span data-ttu-id="dec8a-232">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dec8a-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="dec8a-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="dec8a-234">Click **Add** button.</span></span> <span data-ttu-id="dec8a-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dec8a-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="dec8a-237">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="dec8a-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dec8a-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dec8a-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dec8a-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dec8a-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dec8a-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dec8a-240">Testing single sign-on</span></span>

<span data-ttu-id="dec8a-241">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dec8a-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dec8a-242">När du klickar på Dropbox för företag-panelen på åtkomstpanelen bör du hämta inloggningssidan i din Dropbox för affärsprogram.</span><span class="sxs-lookup"><span data-stu-id="dec8a-242">When you click the Dropbox for Business tile in the Access Panel, you should get login page of your Dropbox for Business application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dec8a-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="dec8a-243">Additional resources</span></span>

* [<span data-ttu-id="dec8a-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dec8a-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dec8a-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dec8a-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="dec8a-246">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="dec8a-246">Configure User Provisioning</span></span>](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

