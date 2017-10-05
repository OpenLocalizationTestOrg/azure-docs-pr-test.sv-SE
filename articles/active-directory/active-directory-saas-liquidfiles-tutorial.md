---
title: "Självstudier: Azure Active Directory-integrering med LiquidFiles | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LiquidFiles."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: b858c6d26b78be4641a46b3453f53d103b512356
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a><span data-ttu-id="99730-103">Självstudier: Azure Active Directory-integrering med LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="99730-103">Tutorial: Azure Active Directory integration with LiquidFiles</span></span>

<span data-ttu-id="99730-104">I kursen får lära du att integrera LiquidFiles med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="99730-104">In this tutorial, you learn how to integrate LiquidFiles with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99730-105">Integrera LiquidFiles med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="99730-105">Integrating LiquidFiles with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="99730-106">Du kan styra i Azure AD som har åtkomst till LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="99730-106">You can control in Azure AD who has access to LiquidFiles</span></span>
- <span data-ttu-id="99730-107">Du kan aktivera användarna att automatiskt hämta loggat in på LiquidFiles (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="99730-107">You can enable your users to automatically get signed-on to LiquidFiles (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="99730-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="99730-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="99730-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99730-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99730-110">Krav</span><span class="sxs-lookup"><span data-stu-id="99730-110">Prerequisites</span></span>

<span data-ttu-id="99730-111">För att konfigurera Azure AD-integrering med LiquidFiles, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="99730-111">To configure Azure AD integration with LiquidFiles, you need the following items:</span></span>

- <span data-ttu-id="99730-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="99730-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99730-113">En LiquidFiles enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="99730-113">A LiquidFiles single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="99730-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="99730-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="99730-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="99730-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99730-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="99730-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="99730-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99730-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="99730-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="99730-118">Scenario description</span></span>
<span data-ttu-id="99730-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="99730-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99730-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="99730-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99730-121">Att lägga till LiquidFiles från galleriet</span><span class="sxs-lookup"><span data-stu-id="99730-121">Adding LiquidFiles from the gallery</span></span>
2. <span data-ttu-id="99730-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99730-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-liquidfiles-from-the-gallery"></a><span data-ttu-id="99730-123">Att lägga till LiquidFiles från galleriet</span><span class="sxs-lookup"><span data-stu-id="99730-123">Adding LiquidFiles from the gallery</span></span>
<span data-ttu-id="99730-124">Du måste lägga till LiquidFiles från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LiquidFiles i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99730-124">To configure the integration of LiquidFiles into Azure AD, you need to add LiquidFiles from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="99730-125">**Utför följande steg för att lägga till LiquidFiles från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="99730-125">**To add LiquidFiles from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="99730-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99730-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="99730-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="99730-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="99730-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99730-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="99730-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99730-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="99730-133">I sökrutan skriver **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="99730-133">In the search box, type **LiquidFiles**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. <span data-ttu-id="99730-135">Välj i resultatpanelen **LiquidFiles**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="99730-135">In the results panel, select **LiquidFiles**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="99730-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99730-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="99730-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LiquidFiles baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="99730-138">In this section, you configure and test Azure AD single sign-on with LiquidFiles based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="99730-139">Azure AD måste du känna till användaren i LiquidFiles motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="99730-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LiquidFiles is to a user in Azure AD.</span></span> <span data-ttu-id="99730-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LiquidFiles upprättas.</span><span class="sxs-lookup"><span data-stu-id="99730-140">In other words, a link relationship between an Azure AD user and the related user in LiquidFiles needs to be established.</span></span>

<span data-ttu-id="99730-141">I LiquidFiles, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="99730-141">In LiquidFiles, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="99730-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LiquidFiles, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="99730-142">To configure and test Azure AD single sign-on with LiquidFiles, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="99730-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="99730-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="99730-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99730-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99730-145">**[Skapa en testanvändare LiquidFiles](#creating-a-liquidfiles-test-user)**  – du har en motsvarighet för Britta Simon i LiquidFiles som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="99730-145">**[Creating a LiquidFiles test user](#creating-a-liquidfiles-test-user)** - to have a counterpart of Britta Simon in LiquidFiles that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="99730-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99730-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99730-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="99730-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="99730-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99730-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="99730-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LiquidFiles program.</span><span class="sxs-lookup"><span data-stu-id="99730-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LiquidFiles application.</span></span>

<span data-ttu-id="99730-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LiquidFiles:**</span><span class="sxs-lookup"><span data-stu-id="99730-150">**To configure Azure AD single sign-on with LiquidFiles, perform the following steps:**</span></span>

1. <span data-ttu-id="99730-151">I Azure-portalen på den **LiquidFiles** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="99730-151">In the Azure portal, on the **LiquidFiles** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="99730-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99730-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. <span data-ttu-id="99730-155">På den **LiquidFiles domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="99730-155">On the **LiquidFiles Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    <span data-ttu-id="99730-157">a.</span><span class="sxs-lookup"><span data-stu-id="99730-157">a.</span></span> <span data-ttu-id="99730-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<YOUR_SERVER_URL>/saml/init`</span><span class="sxs-lookup"><span data-stu-id="99730-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>/saml/init`</span></span>

    <span data-ttu-id="99730-159">b.</span><span class="sxs-lookup"><span data-stu-id="99730-159">b.</span></span> <span data-ttu-id="99730-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<YOUR_SERVER_URL>`</span><span class="sxs-lookup"><span data-stu-id="99730-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>`</span></span>

    <span data-ttu-id="99730-161">c.</span><span class="sxs-lookup"><span data-stu-id="99730-161">c.</span></span> <span data-ttu-id="99730-162">b.</span><span class="sxs-lookup"><span data-stu-id="99730-162">b.</span></span> <span data-ttu-id="99730-163">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<YOUR_SERVER_URL>/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="99730-163">In the **Reply URL** textbox, type a URL using the following pattern: `https://<YOUR_SERVER_URL>/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="99730-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="99730-164">These values are not real.</span></span> <span data-ttu-id="99730-165">Uppdatera dessa värden med den faktiska inloggnings-URL, identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="99730-165">Update these values with the actual Sign-On URL, Identifier and, Reply URL.</span></span> <span data-ttu-id="99730-166">Kontakta [LiquidFiles klienten supportteamet](https://www.liquidfiles.com/support.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="99730-166">Contact [LiquidFiles Client support team](https://www.liquidfiles.com/support.html) to get these values.</span></span> 
 
4. <span data-ttu-id="99730-167">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="99730-167">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. <span data-ttu-id="99730-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99730-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="99730-171">På den **LiquidFiles Configuration** klickar du på **konfigurera LiquidFiles** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="99730-171">On the **LiquidFiles Configuration** section, click **Configure LiquidFiles** to open **Configure sign-on** window.</span></span> <span data-ttu-id="99730-172">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="99730-172">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. <span data-ttu-id="99730-174">Inloggning på webbplatsen LiquidFiles företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="99730-174">Sign-on to your LiquidFiles company site as administrator.</span></span>

8. <span data-ttu-id="99730-175">Klicka på **enkel inloggning** i den **Admin > Configuration** på menyn.</span><span class="sxs-lookup"><span data-stu-id="99730-175">Click **Single Sign-On** in the **Admin > Configuration** from the menu.</span></span>

9. <span data-ttu-id="99730-176">På den **konfiguration för enkel inloggning** utför följande steg</span><span class="sxs-lookup"><span data-stu-id="99730-176">On the **Single Sign-On Configuration** page, perform the following steps</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    <span data-ttu-id="99730-178">a.</span><span class="sxs-lookup"><span data-stu-id="99730-178">a.</span></span> <span data-ttu-id="99730-179">Som **enkel inloggning på metoden**väljer **SAML 2**.</span><span class="sxs-lookup"><span data-stu-id="99730-179">As **Single Sign On Method**, select **SAML 2**.</span></span>

    <span data-ttu-id="99730-180">b.</span><span class="sxs-lookup"><span data-stu-id="99730-180">b.</span></span> <span data-ttu-id="99730-181">I den **IDP inloggnings-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="99730-181">In the **IDP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="99730-182">c.</span><span class="sxs-lookup"><span data-stu-id="99730-182">c.</span></span> <span data-ttu-id="99730-183">I den **IDP logga ut URL** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="99730-183">In the **IDP Logout URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="99730-184">d.</span><span class="sxs-lookup"><span data-stu-id="99730-184">d.</span></span> <span data-ttu-id="99730-185">I den **IDP Cert fingeravtryck** textruta klistra in den **TUMAVTRYCKET** värde som du har kopierat från Azure-portalen...</span><span class="sxs-lookup"><span data-stu-id="99730-185">In the **IDP Cert Fingerprint** textbox, paste the **THUMBPRINT** value which you have copied from Azure portal..</span></span>

    <span data-ttu-id="99730-186">e.</span><span class="sxs-lookup"><span data-stu-id="99730-186">e.</span></span> <span data-ttu-id="99730-187">Ange värdet i textrutan identifierare namnformat **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="99730-187">In the Name Identifier Format textbox, type the value **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="99730-188">f.</span><span class="sxs-lookup"><span data-stu-id="99730-188">f.</span></span> <span data-ttu-id="99730-189">Skriv ett värde i textrutan Authn kontexten **urn: oasis: namn: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.</span><span class="sxs-lookup"><span data-stu-id="99730-189">In the Authn Context textbox, type the value **urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport**.</span></span>

    <span data-ttu-id="99730-190">g.</span><span class="sxs-lookup"><span data-stu-id="99730-190">g.</span></span> <span data-ttu-id="99730-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="99730-191">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="99730-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="99730-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="99730-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="99730-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="99730-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="99730-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="99730-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="99730-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="99730-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99730-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="99730-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="99730-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="99730-199">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99730-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="99730-201">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="99730-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="99730-203">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99730-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="99730-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="99730-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="99730-207">a.</span><span class="sxs-lookup"><span data-stu-id="99730-207">a.</span></span> <span data-ttu-id="99730-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99730-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99730-209">b.</span><span class="sxs-lookup"><span data-stu-id="99730-209">b.</span></span> <span data-ttu-id="99730-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="99730-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="99730-211">c.</span><span class="sxs-lookup"><span data-stu-id="99730-211">c.</span></span> <span data-ttu-id="99730-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="99730-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="99730-213">d.</span><span class="sxs-lookup"><span data-stu-id="99730-213">d.</span></span> <span data-ttu-id="99730-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99730-214">Click **Create**.</span></span>
 
### <a name="creating-a-liquidfiles-test-user"></a><span data-ttu-id="99730-215">Skapa en testanvändare LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="99730-215">Creating a LiquidFiles test user</span></span>

<span data-ttu-id="99730-216">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="99730-216">The objective of this section is to create a user called Britta Simon in LiquidFiles.</span></span> <span data-ttu-id="99730-217">Arbeta med serveradministratören LiquidFiles få själv läggas till som en användare innan du loggar in i tillämpningsprogrammet LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="99730-217">Work with your LiquidFiles server administrator to get yourself added as a user before logging in to your LiquidFiles application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="99730-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="99730-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="99730-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="99730-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LiquidFiles.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="99730-221">**Om du vill tilldela LiquidFiles Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="99730-221">**To assign Britta Simon to LiquidFiles, perform the following steps:**</span></span>

1. <span data-ttu-id="99730-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99730-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="99730-224">Välj i listan med program **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="99730-224">In the applications list, select **LiquidFiles**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. <span data-ttu-id="99730-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="99730-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="99730-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="99730-228">Click **Add** button.</span></span> <span data-ttu-id="99730-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99730-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="99730-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="99730-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="99730-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99730-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99730-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99730-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="99730-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99730-234">Testing single sign-on</span></span>

<span data-ttu-id="99730-235">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="99730-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="99730-236">När du klickar på panelen LiquidFiles på åtkomstpanelen du bör få automatiskt loggat in på ditt LiquidFiles program.</span><span class="sxs-lookup"><span data-stu-id="99730-236">When you click the LiquidFiles tile in the Access Panel, you should get automatically signed-on to your LiquidFiles application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99730-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="99730-237">Additional resources</span></span>

* [<span data-ttu-id="99730-238">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99730-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99730-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99730-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

