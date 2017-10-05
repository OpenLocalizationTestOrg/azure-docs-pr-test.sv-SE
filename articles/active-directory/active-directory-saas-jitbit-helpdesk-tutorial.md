---
title: "Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Jitbit Helpdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 6523ee3179dafd79528093b856b0ec10fafb4f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="97dc5-103">Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="97dc5-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="97dc5-104">I kursen får lära du att integrera Jitbit Helpdesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="97dc5-104">In this tutorial, you learn how to integrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97dc5-105">Integrera Jitbit Helpdesk med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="97dc5-105">Integrating Jitbit Helpdesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="97dc5-106">Du kan styra i Azure AD som har åtkomst till Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="97dc5-106">You can control in Azure AD who has access to Jitbit Helpdesk</span></span>
- <span data-ttu-id="97dc5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Jitbit Helpdesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="97dc5-107">You can enable your users to automatically get signed-on to Jitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97dc5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97dc5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="97dc5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97dc5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97dc5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="97dc5-110">Prerequisites</span></span>

<span data-ttu-id="97dc5-111">För att konfigurera Azure AD-integrering med Jitbit Helpdesk, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="97dc5-111">To configure Azure AD integration with Jitbit Helpdesk, you need the following items:</span></span>

- <span data-ttu-id="97dc5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="97dc5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="97dc5-113">En Jitbit Helpdesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="97dc5-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97dc5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="97dc5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97dc5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="97dc5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97dc5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="97dc5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97dc5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97dc5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97dc5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="97dc5-118">Scenario description</span></span>
<span data-ttu-id="97dc5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="97dc5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97dc5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="97dc5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97dc5-121">Att lägga till Jitbit Helpdesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="97dc5-121">Adding Jitbit Helpdesk from the gallery</span></span>
2. <span data-ttu-id="97dc5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97dc5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a><span data-ttu-id="97dc5-123">Att lägga till Jitbit Helpdesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="97dc5-123">Adding Jitbit Helpdesk from the gallery</span></span>
<span data-ttu-id="97dc5-124">Du måste lägga till Jitbit Helpdesk från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Jitbit Helpdesk i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97dc5-124">To configure the integration of Jitbit Helpdesk into Azure AD, you need to add Jitbit Helpdesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="97dc5-125">**Utför följande steg för att lägga till Jitbit Helpdesk från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="97dc5-125">**To add Jitbit Helpdesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="97dc5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="97dc5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97dc5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="97dc5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="97dc5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97dc5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="97dc5-133">I sökrutan skriver **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-133">In the search box, type **Jitbit Helpdesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="97dc5-135">Välj i resultatpanelen **Jitbit Helpdesk**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="97dc5-135">In the results panel, select **Jitbit Helpdesk**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97dc5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97dc5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97dc5-138">Du konfigurera och testa Azure AD enkel inloggning med Jitbit Helpdesk baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="97dc5-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="97dc5-139">Azure AD måste du känna till användaren i Jitbit Helpdesk motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="97dc5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jitbit Helpdesk is to a user in Azure AD.</span></span> <span data-ttu-id="97dc5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Jitbit Helpdesk upprättas.</span><span class="sxs-lookup"><span data-stu-id="97dc5-140">In other words, a link relationship between an Azure AD user and the related user in Jitbit Helpdesk needs to be established.</span></span>

<span data-ttu-id="97dc5-141">I Jitbit Helpdesk tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-141">In Jitbit Helpdesk, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="97dc5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Jitbit Helpdesk, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="97dc5-142">To configure and test Azure AD single sign-on with Jitbit Helpdesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="97dc5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="97dc5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97dc5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97dc5-145">**[Skapa en testanvändare Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  – har en motsvarighet för Britta Simon Jitbit Helpdesk som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="97dc5-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - to have a counterpart of Britta Simon in Jitbit Helpdesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="97dc5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="97dc5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97dc5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="97dc5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97dc5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97dc5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97dc5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="97dc5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="97dc5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Jitbit Helpdesk:**</span><span class="sxs-lookup"><span data-stu-id="97dc5-150">**To configure Azure AD single sign-on with Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="97dc5-151">I Azure-portalen på den **Jitbit Helpdesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-151">In the Azure portal, on the **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="97dc5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="97dc5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="97dc5-155">På den **Jitbit supportavdelningen domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="97dc5-155">On the **Jitbit Helpdesk Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="97dc5-157">a.</span><span class="sxs-lookup"><span data-stu-id="97dc5-157">a.</span></span> <span data-ttu-id="97dc5-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="97dc5-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="97dc5-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="97dc5-159">This value is not real.</span></span> <span data-ttu-id="97dc5-160">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="97dc5-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="97dc5-161">Kontakta [Jitbit supportavdelningen klienten supportteamet](https://www.jitbit.com/support/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="97dc5-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) to get this value.</span></span> 
    
    <span data-ttu-id="97dc5-162">b.</span><span class="sxs-lookup"><span data-stu-id="97dc5-162">b.</span></span>  <span data-ttu-id="97dc5-163">I den **identifierare** textruta Skriv en URL som följande:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="97dc5-163">In the **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="97dc5-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="97dc5-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="97dc5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="97dc5-168">På den **Jitbit supportavdelningen Configuration** klickar du på **konfigurera Jitbit supportavdelningen** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="97dc5-168">On the **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="97dc5-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="97dc5-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="97dc5-171">Logga in på webbplatsen Jitbit Helpdesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="97dc5-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="97dc5-172">Klicka på i verktygsfältet högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-172">In the toolbar on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="97dc5-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="97dc5-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="97dc5-174">Klicka på **allmänna inställningar**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="97dc5-175">![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "användare, företag och behörigheter")</span><span class="sxs-lookup"><span data-stu-id="97dc5-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="97dc5-176">I den **autentiseringsinställningar** konfiguration och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="97dc5-176">In the **Authentication settings** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="97dc5-177">![Autentiseringsinställningar](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "autentiseringsinställningar")</span><span class="sxs-lookup"><span data-stu-id="97dc5-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="97dc5-178">a.</span><span class="sxs-lookup"><span data-stu-id="97dc5-178">a.</span></span> <span data-ttu-id="97dc5-179">Välj **aktivera SAML 2.0 enkel inloggning**, för att logga in med enkel inloggning (SSO), **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-179">Select **Enable SAML 2.0 single sign on**, to sign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="97dc5-180">b.</span><span class="sxs-lookup"><span data-stu-id="97dc5-180">b.</span></span> <span data-ttu-id="97dc5-181">I den **slutpunkts-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-181">In the **EndPoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="97dc5-182">c.</span><span class="sxs-lookup"><span data-stu-id="97dc5-182">c.</span></span> <span data-ttu-id="97dc5-183">Öppna din **Base64-** kodat certifikat i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="97dc5-183">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="97dc5-184">d.</span><span class="sxs-lookup"><span data-stu-id="97dc5-184">d.</span></span> <span data-ttu-id="97dc5-185">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="97dc5-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="97dc5-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="97dc5-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="97dc5-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="97dc5-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97dc5-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97dc5-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="97dc5-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="97dc5-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97dc5-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="97dc5-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="97dc5-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="97dc5-193">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="97dc5-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97dc5-195">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97dc5-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97dc5-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97dc5-199">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="97dc5-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97dc5-201">a.</span><span class="sxs-lookup"><span data-stu-id="97dc5-201">a.</span></span> <span data-ttu-id="97dc5-202">I den **namn** textruta typnamn som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-202">In the **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="97dc5-203">b.</span><span class="sxs-lookup"><span data-stu-id="97dc5-203">b.</span></span> <span data-ttu-id="97dc5-204">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97dc5-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97dc5-205">c.</span><span class="sxs-lookup"><span data-stu-id="97dc5-205">c.</span></span> <span data-ttu-id="97dc5-206">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="97dc5-207">d.</span><span class="sxs-lookup"><span data-stu-id="97dc5-207">d.</span></span> <span data-ttu-id="97dc5-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="97dc5-209">Skapa en testanvändare Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="97dc5-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="97dc5-210">För att aktivera Azure AD-användare att logga in på Jitbit Helpdesk etableras de i Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="97dc5-210">In order to enable Azure AD users to log into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="97dc5-211">När det gäller Jitbit Helpdesk är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="97dc5-211">In the case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="97dc5-212">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="97dc5-212">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="97dc5-213">Logga in på ditt **Jitbit Helpdesk** klient.</span><span class="sxs-lookup"><span data-stu-id="97dc5-213">Log in to your **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="97dc5-214">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-214">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="97dc5-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="97dc5-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="97dc5-216">Klicka på **användare och behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="97dc5-217">![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "användare, företag och behörigheter")</span><span class="sxs-lookup"><span data-stu-id="97dc5-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="97dc5-218">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="97dc5-219">![Lägg till användare](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="97dc5-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="97dc5-220">I avsnittet Skapa Skriv data i Azure AD-kontot som du vill etablera på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="97dc5-220">In the Create section, type the data of the Azure AD account you want to provision as follows:</span></span>

    <span data-ttu-id="97dc5-221">![Skapa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="97dc5-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="97dc5-222">a.</span><span class="sxs-lookup"><span data-stu-id="97dc5-222">a.</span></span> <span data-ttu-id="97dc5-223">I den **användarnamn** textruta typen **BrittaSimon**, användarnamnet som Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-223">In the **Username** textbox, type **BrittaSimon**, the user name as in the Azure portal.</span></span>

   <span data-ttu-id="97dc5-224">b.</span><span class="sxs-lookup"><span data-stu-id="97dc5-224">b.</span></span> <span data-ttu-id="97dc5-225">I den **e-post** textruta e-post för användaren som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="97dc5-225">In the **Email** textbox, type email of the user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="97dc5-226">c.</span><span class="sxs-lookup"><span data-stu-id="97dc5-226">c.</span></span> <span data-ttu-id="97dc5-227">I den **Förnamn** textruta första typnamnet för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-227">In the **First Name** textbox, type first name of the user like **Britta**.</span></span>

   <span data-ttu-id="97dc5-228">d.</span><span class="sxs-lookup"><span data-stu-id="97dc5-228">d.</span></span> <span data-ttu-id="97dc5-229">I den **efternamn** textruta Skriv Efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-229">In the **Last Name** textbox, type last name of the user like **Simon**.</span></span>
   
   <span data-ttu-id="97dc5-230">e.</span><span class="sxs-lookup"><span data-stu-id="97dc5-230">e.</span></span> <span data-ttu-id="97dc5-231">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="97dc5-232">Du kan använda andra Jitbit Helpdesk användare skapa verktyg eller API: er som tillhandahålls av Jitbit Helpdesk för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="97dc5-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk to provision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="97dc5-233">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="97dc5-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="97dc5-234">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="97dc5-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jitbit Helpdesk.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="97dc5-236">**Om du vill tilldela Jitbit Helpdesk Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="97dc5-236">**To assign Britta Simon to Jitbit Helpdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="97dc5-237">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="97dc5-239">Välj i listan med program **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-239">In the applications list, select **Jitbit Helpdesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="97dc5-241">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="97dc5-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="97dc5-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-243">Click **Add** button.</span></span> <span data-ttu-id="97dc5-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97dc5-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="97dc5-246">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="97dc5-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="97dc5-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97dc5-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97dc5-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="97dc5-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97dc5-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="97dc5-249">Testing single sign-on</span></span>

<span data-ttu-id="97dc5-250">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="97dc5-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="97dc5-251">Du bör få inloggningssidan för Jitbit Helpdesk program när du klickar på panelen Jitbit Helpdesk på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="97dc5-251">When you click the Jitbit Helpdesk tile in the Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="97dc5-252">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="97dc5-252">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97dc5-253">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="97dc5-253">Additional resources</span></span>

* [<span data-ttu-id="97dc5-254">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97dc5-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97dc5-255">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97dc5-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

