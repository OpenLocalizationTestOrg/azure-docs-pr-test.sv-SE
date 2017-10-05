---
title: "Självstudier: Azure Active Directory-integrering med AnswerHub | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och AnswerHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a1c9cc5d7a2ebe28e9fb7e0e6ed8e3d393873ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a><span data-ttu-id="38088-103">Självstudier: Azure Active Directory-integrering med AnswerHub</span><span class="sxs-lookup"><span data-stu-id="38088-103">Tutorial: Azure Active Directory integration with AnswerHub</span></span>

<span data-ttu-id="38088-104">I kursen får lära du att integrera AnswerHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="38088-104">In this tutorial, you learn how to integrate AnswerHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="38088-105">Integrera AnswerHub med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="38088-105">Integrating AnswerHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="38088-106">Du kan styra i Azure AD som har åtkomst till AnswerHub</span><span class="sxs-lookup"><span data-stu-id="38088-106">You can control in Azure AD who has access to AnswerHub</span></span>
- <span data-ttu-id="38088-107">Du kan aktivera användarna att automatiskt hämta loggat in på AnswerHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="38088-107">You can enable your users to automatically get signed-on to AnswerHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="38088-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="38088-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="38088-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="38088-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38088-110">Krav</span><span class="sxs-lookup"><span data-stu-id="38088-110">Prerequisites</span></span>

<span data-ttu-id="38088-111">För att konfigurera Azure AD-integrering med AnswerHub, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="38088-111">To configure Azure AD integration with AnswerHub, you need the following items:</span></span>

- <span data-ttu-id="38088-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="38088-112">An Azure AD subscription</span></span>
- <span data-ttu-id="38088-113">En AnswerHub enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="38088-113">An AnswerHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="38088-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="38088-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="38088-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="38088-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="38088-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="38088-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="38088-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="38088-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="38088-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="38088-118">Scenario description</span></span>
<span data-ttu-id="38088-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="38088-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="38088-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="38088-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="38088-121">Att lägga till AnswerHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="38088-121">Adding AnswerHub from the gallery</span></span>
2. <span data-ttu-id="38088-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="38088-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-answerhub-from-the-gallery"></a><span data-ttu-id="38088-123">Att lägga till AnswerHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="38088-123">Adding AnswerHub from the gallery</span></span>
<span data-ttu-id="38088-124">Du måste lägga till AnswerHub från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av AnswerHub i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="38088-124">To configure the integration of AnswerHub into Azure AD, you need to add AnswerHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="38088-125">**Utför följande steg för att lägga till AnswerHub från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="38088-125">**To add AnswerHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="38088-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="38088-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="38088-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="38088-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="38088-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="38088-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="38088-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="38088-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="38088-133">I sökrutan skriver **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="38088-133">In the search box, type **AnswerHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. <span data-ttu-id="38088-135">Välj i resultatpanelen **AnswerHub**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="38088-135">In the results panel, select **AnswerHub**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="38088-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="38088-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="38088-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AnswerHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="38088-138">In this section, you configure and test Azure AD single sign-on with AnswerHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="38088-139">Azure AD måste du känna till användaren i AnswerHub motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="38088-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AnswerHub is to a user in Azure AD.</span></span> <span data-ttu-id="38088-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i AnswerHub upprättas.</span><span class="sxs-lookup"><span data-stu-id="38088-140">In other words, a link relationship between an Azure AD user and the related user in AnswerHub needs to be established.</span></span>

<span data-ttu-id="38088-141">I AnswerHub, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="38088-141">In AnswerHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="38088-142">Om du vill konfigurera och testa Azure AD enkel inloggning med AnswerHub, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="38088-142">To configure and test Azure AD single sign-on with AnswerHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="38088-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="38088-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="38088-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="38088-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="38088-145">**[Skapa en testanvändare AnswerHub](#creating-an-answerhub-test-user)**  – du har en motsvarighet för Britta Simon i AnswerHub som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="38088-145">**[Creating an AnswerHub test user](#creating-an-answerhub-test-user)** - to have a counterpart of Britta Simon in AnswerHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="38088-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="38088-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="38088-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="38088-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="38088-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="38088-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="38088-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt AnswerHub program.</span><span class="sxs-lookup"><span data-stu-id="38088-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AnswerHub application.</span></span>

<span data-ttu-id="38088-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med AnswerHub:**</span><span class="sxs-lookup"><span data-stu-id="38088-150">**To configure Azure AD single sign-on with AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="38088-151">I Azure-portalen på den **AnswerHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="38088-151">In the Azure portal, on the **AnswerHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="38088-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="38088-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. <span data-ttu-id="38088-155">På den **AnswerHub domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="38088-155">On the **AnswerHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    <span data-ttu-id="38088-157">a.</span><span class="sxs-lookup"><span data-stu-id="38088-157">a.</span></span> <span data-ttu-id="38088-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="38088-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    <span data-ttu-id="38088-159">b.</span><span class="sxs-lookup"><span data-stu-id="38088-159">b.</span></span> <span data-ttu-id="38088-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="38088-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company>.answerhub.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="38088-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="38088-161">These values are not real.</span></span> <span data-ttu-id="38088-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="38088-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="38088-163">Kontakta [AnswerHub klienten supportteamet](mailto:success@answerhub.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="38088-163">Contact [AnswerHub Client support team](mailto:success@answerhub.com) to get these values.</span></span> 
 
4. <span data-ttu-id="38088-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="38088-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. <span data-ttu-id="38088-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="38088-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="38088-168">På den **AnswerHub Configuration** klickar du på **konfigurera AnswerHub** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="38088-168">On the **AnswerHub Configuration** section, click **Configure AnswerHub** to open **Configure sign-on** window.</span></span> <span data-ttu-id="38088-169">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="38088-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. <span data-ttu-id="38088-171">Logga in på webbplatsen AnswerHub företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="38088-171">In a different web browser window, log into your AnswerHub company site as an administrator.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="38088-172">Om du behöver hjälp med att konfigurera AnswerHub kontaktar [Answerhubs supportteamet](mailto:success@answerhub.com.).</span><span class="sxs-lookup"><span data-stu-id="38088-172">If you need help configuring AnswerHub, contact [AnswerHub's support team](mailto:success@answerhub.com.).</span></span>
   
8. <span data-ttu-id="38088-173">Gå till **Administration**.</span><span class="sxs-lookup"><span data-stu-id="38088-173">Go to **Administration**.</span></span>

9. <span data-ttu-id="38088-174">Klicka på den **användar- och** fliken.</span><span class="sxs-lookup"><span data-stu-id="38088-174">Click the **User and Group** tab.</span></span>

10. <span data-ttu-id="38088-175">I navigeringsfönstret till vänster i den **sociala inställningar** klickar du på **SAML installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="38088-175">In the navigation pane on the left side, in the **Social Settings** section, click **SAML Setup**.</span></span>

11. <span data-ttu-id="38088-176">Klicka på **IDP Config** fliken.</span><span class="sxs-lookup"><span data-stu-id="38088-176">Click **IDP Config** tab.</span></span>

12. <span data-ttu-id="38088-177">På den **IDP Config** fliken, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="38088-177">On the **IDP Config** tab, perform the following steps:</span></span>

     <span data-ttu-id="38088-178">![SAML-installationsprogrammet](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="38088-178">![SAML Setup](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")</span></span>  
  
     <span data-ttu-id="38088-179">a.</span><span class="sxs-lookup"><span data-stu-id="38088-179">a.</span></span> <span data-ttu-id="38088-180">I **IDP inloggnings-URL** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="38088-180">In **IDP Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
     <span data-ttu-id="38088-181">b.</span><span class="sxs-lookup"><span data-stu-id="38088-181">b.</span></span> <span data-ttu-id="38088-182">I **IDP logga ut URL** textruta klistra in **Sign-Out URL** värde som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="38088-182">In **IDP Logout URL** textbox, paste **Sign-Out URL** value which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="38088-183">c.</span><span class="sxs-lookup"><span data-stu-id="38088-183">c.</span></span> <span data-ttu-id="38088-184">I **IDP namnformat identifierare** textruta ange användaren identifierare värdet samma som väljs i Azure-portalen i **användarattribut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="38088-184">In **IDP Name Identifier Format** textbox, enter the user Identifier value same as selected in Azure portal in **User Attributes** section.</span></span>
  
     <span data-ttu-id="38088-185">d.</span><span class="sxs-lookup"><span data-stu-id="38088-185">d.</span></span> <span data-ttu-id="38088-186">Klicka på **nycklar och certifikat**.</span><span class="sxs-lookup"><span data-stu-id="38088-186">Click **Keys and Certificates**.</span></span>

13. <span data-ttu-id="38088-187">Utför följande steg på fliken nycklar och certifikat:</span><span class="sxs-lookup"><span data-stu-id="38088-187">On the Keys and Certificates tab, perform the following steps:</span></span>
    
     <span data-ttu-id="38088-188">![Nycklar och certifikat](./media/active-directory-saas-answerhub-tutorial/ic785173.png "nycklar och certifikat")</span><span class="sxs-lookup"><span data-stu-id="38088-188">![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Keys and Certificates")</span></span>  
 
     <span data-ttu-id="38088-189">a.</span><span class="sxs-lookup"><span data-stu-id="38088-189">a.</span></span> <span data-ttu-id="38088-190">Öppna din Base64-kodade certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **IDP offentlig nyckel (x 509-Format)** textruta.</span><span class="sxs-lookup"><span data-stu-id="38088-190">Open your base-64 encoded certificate which you have downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **IDP Public Key (x509 Format)** textbox.</span></span>
  
     <span data-ttu-id="38088-191">b.</span><span class="sxs-lookup"><span data-stu-id="38088-191">b.</span></span> <span data-ttu-id="38088-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="38088-192">Click **Save**.</span></span>

14. <span data-ttu-id="38088-193">På den **IDP Config** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="38088-193">On the **IDP Config** tab, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="38088-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="38088-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="38088-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="38088-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="38088-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="38088-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="38088-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="38088-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="38088-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="38088-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="38088-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="38088-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="38088-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="38088-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="38088-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="38088-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="38088-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="38088-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="38088-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="38088-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="38088-209">a.</span><span class="sxs-lookup"><span data-stu-id="38088-209">a.</span></span> <span data-ttu-id="38088-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="38088-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="38088-211">b.</span><span class="sxs-lookup"><span data-stu-id="38088-211">b.</span></span> <span data-ttu-id="38088-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="38088-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="38088-213">c.</span><span class="sxs-lookup"><span data-stu-id="38088-213">c.</span></span> <span data-ttu-id="38088-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="38088-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="38088-215">d.</span><span class="sxs-lookup"><span data-stu-id="38088-215">d.</span></span> <span data-ttu-id="38088-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="38088-216">Click **Create**.</span></span>
 
### <a name="creating-an-answerhub-test-user"></a><span data-ttu-id="38088-217">Skapa en testanvändare AnswerHub</span><span class="sxs-lookup"><span data-stu-id="38088-217">Creating an AnswerHub test user</span></span>

<span data-ttu-id="38088-218">Om du vill aktivera Azure AD-användare kan logga in på AnswerHub etableras de i AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="38088-218">To enable Azure AD users to log in to AnswerHub, they must be provisioned into AnswerHub.</span></span>  
<span data-ttu-id="38088-219">När det gäller AnswerHub är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="38088-219">In the case of AnswerHub, provisioning is a manual task.</span></span>

<span data-ttu-id="38088-220">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="38088-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="38088-221">Logga in på ditt **AnswerHub** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="38088-221">Log in to your **AnswerHub** company site as administrator.</span></span>

2. <span data-ttu-id="38088-222">Gå till **Administration**.</span><span class="sxs-lookup"><span data-stu-id="38088-222">Go to **Administration**.</span></span>

3. <span data-ttu-id="38088-223">Klicka på den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="38088-223">Click the **Users & Groups** tab.</span></span>

4. <span data-ttu-id="38088-224">I navigeringsfönstret till vänster i den **hantera användare** klickar du på **skapa eller importera användare**.</span><span class="sxs-lookup"><span data-stu-id="38088-224">In the navigation pane on the left side, in the **Manage Users** section, click **Create or import users**.</span></span>
   
   <span data-ttu-id="38088-225">![Användare och grupper](./media/active-directory-saas-answerhub-tutorial/ic785175.png "användare och grupper")</span><span class="sxs-lookup"><span data-stu-id="38088-225">![Users & Groups](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Users & Groups")</span></span>

5. <span data-ttu-id="38088-226">Typ av **e-postadress**, **användarnamn** och **lösenord** för ett giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor och klicka sedan på  **Spara**.</span><span class="sxs-lookup"><span data-stu-id="38088-226">Type the **Email address**, **Username** and **Password** of a valid Azure Active Directory account you want to provision into the related textboxes, and then click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="38088-227">Du kan använda något annat AnswerHub användarens konto skapas verktyg eller API: er som tillhandahålls av AnswerHub att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="38088-227">You can use any other AnswerHub user account creation tools or APIs provided by AnswerHub to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="38088-228">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="38088-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="38088-229">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="38088-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AnswerHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="38088-231">**Om du vill tilldela AnswerHub Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="38088-231">**To assign Britta Simon to AnswerHub, perform the following steps:**</span></span>

1. <span data-ttu-id="38088-232">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="38088-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="38088-234">Välj i listan med program **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="38088-234">In the applications list, select **AnswerHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. <span data-ttu-id="38088-236">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="38088-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="38088-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="38088-238">Click **Add** button.</span></span> <span data-ttu-id="38088-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="38088-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="38088-241">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="38088-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="38088-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="38088-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="38088-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="38088-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="38088-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="38088-244">Testing single sign-on</span></span>

<span data-ttu-id="38088-245">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="38088-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="38088-246">När du klickar på panelen AnswerHub på åtkomstpanelen du bör få automatiskt loggat in på ditt AnswerHub program.</span><span class="sxs-lookup"><span data-stu-id="38088-246">When you click the AnswerHub tile in the Access Panel, you should get automatically signed-on to your AnswerHub application.</span></span>
<span data-ttu-id="38088-247">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="38088-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38088-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="38088-248">Additional resources</span></span>

* [<span data-ttu-id="38088-249">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="38088-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="38088-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="38088-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

