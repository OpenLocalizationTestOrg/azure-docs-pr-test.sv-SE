---
title: "Självstudier: Azure Active Directory-integrering med Hackerone | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Hackerone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 657d8d4c98b7b133698a5cda0aa675da7f68c464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="df7a5-103">Självstudier: Azure Active Directory-integrering med HackerOne</span><span class="sxs-lookup"><span data-stu-id="df7a5-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="df7a5-104">I kursen får lära du att integrera HackerOne med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="df7a5-104">In this tutorial, you learn how to integrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df7a5-105">Integrera HackerOne med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="df7a5-105">Integrating HackerOne with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="df7a5-106">Du kan styra i Azure AD som har åtkomst till HackerOne</span><span class="sxs-lookup"><span data-stu-id="df7a5-106">You can control in Azure AD who has access to HackerOne</span></span>
- <span data-ttu-id="df7a5-107">Du kan aktivera användarna att automatiskt hämta loggat in på HackerOne (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="df7a5-107">You can enable your users to automatically get signed-on to HackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="df7a5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="df7a5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="df7a5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df7a5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df7a5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="df7a5-110">Prerequisites</span></span>

<span data-ttu-id="df7a5-111">För att konfigurera Azure AD-integrering med HackerOne, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="df7a5-111">To configure Azure AD integration with HackerOne, you need the following items:</span></span>

- <span data-ttu-id="df7a5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="df7a5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="df7a5-113">En HackerOne enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="df7a5-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="df7a5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="df7a5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="df7a5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="df7a5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="df7a5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="df7a5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="df7a5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df7a5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="df7a5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="df7a5-118">Scenario description</span></span>
<span data-ttu-id="df7a5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="df7a5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="df7a5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="df7a5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df7a5-121">Att lägga till HackerOne från galleriet</span><span class="sxs-lookup"><span data-stu-id="df7a5-121">Adding HackerOne from the gallery</span></span>
2. <span data-ttu-id="df7a5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7a5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-the-gallery"></a><span data-ttu-id="df7a5-123">Att lägga till HackerOne från galleriet</span><span class="sxs-lookup"><span data-stu-id="df7a5-123">Adding HackerOne from the gallery</span></span>
<span data-ttu-id="df7a5-124">Du måste lägga till HackerOne från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av HackerOne i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df7a5-124">To configure the integration of HackerOne into Azure AD, you need to add HackerOne from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="df7a5-125">**Utför följande steg för att lägga till HackerOne från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="df7a5-125">**To add HackerOne from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="df7a5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df7a5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="df7a5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="df7a5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="df7a5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7a5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="df7a5-133">I sökrutan skriver **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-133">In the search box, type **HackerOne**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="df7a5-135">Välj i resultatpanelen **HackerOne**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="df7a5-135">In the results panel, select **HackerOne**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="df7a5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7a5-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="df7a5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HackerOne baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="df7a5-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="df7a5-139">Azure AD måste du känna till användaren i HackerOne motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="df7a5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HackerOne is to a user in Azure AD.</span></span> <span data-ttu-id="df7a5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i HackerOne upprättas.</span><span class="sxs-lookup"><span data-stu-id="df7a5-140">In other words, a link relationship between an Azure AD user and the related user in HackerOne needs to be established.</span></span>

<span data-ttu-id="df7a5-141">I HackerOne, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="df7a5-141">In HackerOne, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="df7a5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med HackerOne, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="df7a5-142">To configure and test Azure AD single sign-on with HackerOne, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="df7a5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="df7a5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="df7a5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df7a5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df7a5-145">**[Skapa en testanvändare HackerOne](#creating-a-hackerone-test-user)**  – du har en motsvarighet för Britta Simon i HackerOne som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="df7a5-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - to have a counterpart of Britta Simon in HackerOne that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="df7a5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df7a5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df7a5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="df7a5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="df7a5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7a5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="df7a5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt HackerOne program.</span><span class="sxs-lookup"><span data-stu-id="df7a5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="df7a5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med HackerOne:**</span><span class="sxs-lookup"><span data-stu-id="df7a5-150">**To configure Azure AD single sign-on with HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="df7a5-151">I Azure-portalen på den **HackerOne** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-151">In the Azure portal, on the **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="df7a5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df7a5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="df7a5-155">På den **HackerOne enkel inloggnings-URL och identifierare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7a5-155">On the **HackerOne Single sign-on URL and Identifier** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="df7a5-157">a.</span><span class="sxs-lookup"><span data-stu-id="df7a5-157">a.</span></span> <span data-ttu-id="df7a5-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="df7a5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="df7a5-159">b.</span><span class="sxs-lookup"><span data-stu-id="df7a5-159">b.</span></span> <span data-ttu-id="df7a5-160">I den **identifierare** textruta Skriv en URL som:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="df7a5-160">In the **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="df7a5-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="df7a5-161">This value is not real.</span></span> <span data-ttu-id="df7a5-162">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="df7a5-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="df7a5-163">Kontakta [HackerOne supportteamet](mailto:support@hackerone.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="df7a5-163">Contact [HackerOne support team](mailto:support@hackerone.com) to get this value.</span></span> 
 
4. <span data-ttu-id="df7a5-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="df7a5-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="df7a5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="df7a5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="df7a5-168">På den **HackerOne Configuration** klickar du på **konfigurera HackerOne** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="df7a5-168">On the **HackerOne Configuration** section, click **Configure HackerOne** to open **Configure sign-on** window.</span></span> <span data-ttu-id="df7a5-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="df7a5-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="df7a5-171">Inloggning till HackerOne-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="df7a5-171">Sign On to your HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="df7a5-172">Klicka på menyn högst upp i ”**inställningar**”.</span><span class="sxs-lookup"><span data-stu-id="df7a5-172">In the menu on the top, click the "**Settings**."</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="df7a5-174">Gå till ”**autentisering**” och klicka på ”**Lägg till SAML-inställningar**”.</span><span class="sxs-lookup"><span data-stu-id="df7a5-174">Navigate to "**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="df7a5-176">På den **SAML inställningar** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7a5-176">On the **SAML Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="df7a5-178">a.</span><span class="sxs-lookup"><span data-stu-id="df7a5-178">a.</span></span> <span data-ttu-id="df7a5-179">I den **e-postdomän** textruta skriver en registrerad domän.</span><span class="sxs-lookup"><span data-stu-id="df7a5-179">In the **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="df7a5-180">b.</span><span class="sxs-lookup"><span data-stu-id="df7a5-180">b.</span></span> <span data-ttu-id="df7a5-181">I **enkel inloggning på URL: en** textrutor, klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df7a5-181">In  **Single Sign On URL** textboxes, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="df7a5-182">c.</span><span class="sxs-lookup"><span data-stu-id="df7a5-182">c.</span></span> <span data-ttu-id="df7a5-183">Öppna din **certifikatfilen** i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **X509 certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="df7a5-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="df7a5-184">d.</span><span class="sxs-lookup"><span data-stu-id="df7a5-184">d.</span></span> <span data-ttu-id="df7a5-185">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-185">Click **Save**.</span></span>

11. <span data-ttu-id="df7a5-186">I dialogrutan Inställningar för autentisering utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7a5-186">On the Authentication Settings dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="df7a5-188">a.</span><span class="sxs-lookup"><span data-stu-id="df7a5-188">a.</span></span> <span data-ttu-id="df7a5-189">Klicka på **Kör test**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-189">Click **Run test**.</span></span>

    <span data-ttu-id="df7a5-190">b.</span><span class="sxs-lookup"><span data-stu-id="df7a5-190">b.</span></span> <span data-ttu-id="df7a5-191">Om värdet för den **Status** fältet är lika med **senast teststatus: skapa**, kontakta din [HackerOne supportteamet](mailto:support@hackerone.com) att begära en granskning av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="df7a5-191">If the value of the **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) to request a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="df7a5-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="df7a5-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="df7a5-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="df7a5-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="df7a5-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="df7a5-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="df7a5-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="df7a5-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="df7a5-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df7a5-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="df7a5-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="df7a5-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="df7a5-199">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df7a5-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="df7a5-201">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="df7a5-203">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7a5-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="df7a5-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7a5-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="df7a5-207">a.</span><span class="sxs-lookup"><span data-stu-id="df7a5-207">a.</span></span> <span data-ttu-id="df7a5-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="df7a5-209">b.</span><span class="sxs-lookup"><span data-stu-id="df7a5-209">b.</span></span> <span data-ttu-id="df7a5-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="df7a5-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="df7a5-211">c.</span><span class="sxs-lookup"><span data-stu-id="df7a5-211">c.</span></span> <span data-ttu-id="df7a5-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="df7a5-213">d.</span><span class="sxs-lookup"><span data-stu-id="df7a5-213">d.</span></span> <span data-ttu-id="df7a5-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="df7a5-215">Skapa en testanvändare HackerOne</span><span class="sxs-lookup"><span data-stu-id="df7a5-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="df7a5-216">Därefter skapar du en användare som kallas Britta Simon i HackerOne.</span><span class="sxs-lookup"><span data-stu-id="df7a5-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="df7a5-217">HackerOne stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="df7a5-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="df7a5-218">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="df7a5-218">There is no action item for you in this section.</span></span> <span data-ttu-id="df7a5-219">När du använder HackerOne skapas en ny användare om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="df7a5-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="df7a5-220">Om du behöver skapa en användare manuellt, måste du kontakta supportteamet certifiera.</span><span class="sxs-lookup"><span data-stu-id="df7a5-220">If you need to create a user manually, you need to contact the Certify support team.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="df7a5-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="df7a5-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="df7a5-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till HackerOne.</span><span class="sxs-lookup"><span data-stu-id="df7a5-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HackerOne.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="df7a5-224">**Om du vill tilldela HackerOne Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df7a5-224">**To assign Britta Simon to HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="df7a5-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="df7a5-227">Välj i listan med program **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-227">In the applications list, select **HackerOne**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="df7a5-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="df7a5-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="df7a5-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="df7a5-231">Click **Add** button.</span></span> <span data-ttu-id="df7a5-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7a5-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="df7a5-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="df7a5-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="df7a5-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7a5-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="df7a5-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7a5-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="df7a5-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7a5-237">Testing single sign-on</span></span>

<span data-ttu-id="df7a5-238">Slutligen kan testa du Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="df7a5-238">Finally, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="df7a5-239">När du klickar på panelen HackerOne på åtkomstpanelen du bör få automatiskt loggat in på ditt HackerOne program.</span><span class="sxs-lookup"><span data-stu-id="df7a5-239">When you click the HackerOne tile in the Access Panel, you should get automatically signed-on to your HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df7a5-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="df7a5-240">Additional resources</span></span>

* [<span data-ttu-id="df7a5-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="df7a5-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df7a5-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="df7a5-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

