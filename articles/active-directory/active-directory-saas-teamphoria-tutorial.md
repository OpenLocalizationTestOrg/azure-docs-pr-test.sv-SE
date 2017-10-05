---
title: "Självstudier: Azure Active Directory-integrering med Teamphoria | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Teamphoria."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="ba6d1-103">Självstudier: Azure Active Directory-integrering med Teamphoria</span><span class="sxs-lookup"><span data-stu-id="ba6d1-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="ba6d1-104">I kursen får lära du att integrera Teamphoria med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ba6d1-104">In this tutorial, you learn how to integrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ba6d1-105">Integrera Teamphoria med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-105">Integrating Teamphoria with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ba6d1-106">Du kan styra i Azure AD som har åtkomst till Teamphoria</span><span class="sxs-lookup"><span data-stu-id="ba6d1-106">You can control in Azure AD who has access to Teamphoria</span></span>
- <span data-ttu-id="ba6d1-107">Du kan aktivera användarna att automatiskt hämta loggat in på Teamphoria (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ba6d1-107">You can enable your users to automatically get signed-on to Teamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ba6d1-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="ba6d1-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="ba6d1-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ba6d1-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="ba6d1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ba6d1-110">Prerequisites</span></span>

<span data-ttu-id="ba6d1-111">För att konfigurera Azure AD-integrering med Teamphoria, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-111">To configure Azure AD integration with Teamphoria, you need the following items:</span></span>

- <span data-ttu-id="ba6d1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ba6d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ba6d1-113">En Teamphoria enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ba6d1-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ba6d1-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ba6d1-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ba6d1-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="ba6d1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ba6d1-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ba6d1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ba6d1-118">Scenario description</span></span>
<span data-ttu-id="ba6d1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ba6d1-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ba6d1-121">Att lägga till Teamphoria från galleriet</span><span class="sxs-lookup"><span data-stu-id="ba6d1-121">Adding Teamphoria from the gallery</span></span>
2. <span data-ttu-id="ba6d1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba6d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-the-gallery"></a><span data-ttu-id="ba6d1-123">Att lägga till Teamphoria från galleriet</span><span class="sxs-lookup"><span data-stu-id="ba6d1-123">Adding Teamphoria from the gallery</span></span>
<span data-ttu-id="ba6d1-124">Du måste lägga till Teamphoria från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Teamphoria i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-124">To configure the integration of Teamphoria into Azure AD, you need to add Teamphoria from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ba6d1-125">**Utför följande steg för att lägga till Teamphoria från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-125">**To add Teamphoria from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ba6d1-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ba6d1-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ba6d1-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ba6d1-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ba6d1-133">I sökrutan skriver **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-133">In the search box, type **Teamphoria**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="ba6d1-135">Välj i resultatpanelen **Teamphoria**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-135">In the results panel, select **Teamphoria**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ba6d1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba6d1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ba6d1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Teamphoria baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ba6d1-139">Azure AD måste du känna till användaren i Teamphoria motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Teamphoria is to a user in Azure AD.</span></span> <span data-ttu-id="ba6d1-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Teamphoria upprättas.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-140">In other words, a link relationship between an Azure AD user and the related user in Teamphoria needs to be established.</span></span>

<span data-ttu-id="ba6d1-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Teamphoria.</span></span>

<span data-ttu-id="ba6d1-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Teamphoria, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-142">To configure and test Azure AD single sign-on with Teamphoria, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ba6d1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ba6d1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ba6d1-145">**[Skapa en testanvändare Teamphoria](#creating-a-teamphoria-test-user)**  – du har en motsvarighet för Britta Simon i Teamphoria som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - to have a counterpart of Britta Simon in Teamphoria that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ba6d1-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ba6d1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ba6d1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba6d1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ba6d1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Teamphoria program.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="ba6d1-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Teamphoria:**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-150">**To configure Azure AD single sign-on with Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="ba6d1-151">I Azure-hanteringsportalen på den **Teamphoria** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-151">In the Azure Management portal, on the **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ba6d1-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="ba6d1-155">På den **Teamphoria domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-155">On the **Teamphoria Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="ba6d1-157">a.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-157">a.</span></span> <span data-ttu-id="ba6d1-158">I den **inloggnings-URL** textruta Skriv URL-Adressen med följande mönster:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="ba6d1-158">In the **Sign-on URL** textbox, type the URL using the following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="ba6d1-159">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-159">Please note that these are not the real values.</span></span> <span data-ttu-id="ba6d1-160">Du måste uppdatera dessa värden med den faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-160">You have to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="ba6d1-161">Kontakta [Teamphoria klienten supportteamet](https://www.teamphoria.com/) att hämta Webbadress för inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) to get the Sign-on URL.</span></span> 

4. <span data-ttu-id="ba6d1-162">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatet på datorn.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="ba6d1-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ba6d1-166">På den **Teamphoria Configuration** klickar du på **konfigurera Teamphoria** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-166">On the **Teamphoria Configuration** section, click **Configure Teamphoria** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ba6d1-167">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="ba6d1-169">Konfigurera enkel inloggning på **Teamphoria** sida, logga in i tillämpningsprogrammet Teamphoria som administratör.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-169">To configure single sign-on on **Teamphoria** side, Login to your Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="ba6d1-170">Gå till **ADMINISTRATIONSINSTÄLLNINGAR** alternativet i verktygsfältet till vänster och under den fliken Konfigurera klickar du på **enda SIGN-ON** att öppna fönstret för SSO-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-170">Go to **ADMIN SETTINGS** option in the left toolbar and under the the Configure Tab click on **SINGLE SIGN-ON** to open the SSO configuration window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="ba6d1-172">Klicka på **lägga till nya IDENTITETSLEVERANTÖR** alternativ i det övre högra hörnet för att öppna formuläret för att lägga till inställningarna för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-172">Click on **ADD NEW IDENTITY PROVIDER** option in the top right corner to open the form for adding the settings for SSO.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="ba6d1-174">Ange information i fälten som beskrivs nedan-</span><span class="sxs-lookup"><span data-stu-id="ba6d1-174">Enter the details in the fields as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="ba6d1-176">a.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-176">a.</span></span> <span data-ttu-id="ba6d1-177">**VISNINGSNAMN** : Ange visningsnamnet för plugin-programmet på sidan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-177">**DISPLAY NAME** : Enter the display name of the plugin on the admin page.</span></span>

    <span data-ttu-id="ba6d1-178">b.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-178">b.</span></span> <span data-ttu-id="ba6d1-179">**KNAPPNAMN** : namnet på fliken som visas på inloggningssidan för att logga in via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-179">**BUTTON NAME** : The name of the tab which will display on the login page for logging in via SSO.</span></span>

    <span data-ttu-id="ba6d1-180">c.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-180">c.</span></span> <span data-ttu-id="ba6d1-181">**CERTIFIKATET** : öppna certifikatet tidigare hämtade från Azure-portalen i anteckningar, kopiera innehållet i samma och klistra in den här rutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-181">**CERTIFICATE** : Open the Certificate downloaded earlier from the Azure portal in notepad, copy the contents of the same and paste it here in the box.</span></span>

    <span data-ttu-id="ba6d1-182">d.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-182">d.</span></span> <span data-ttu-id="ba6d1-183">**STARTPUNKTEN** : klistra in den **SAML inloggning tjänst-URL för enkel** kopieras tidigare formuläret Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-183">**ENTRY POINT** : Paste the **SAML Single Sign-On Service URL** copied earlier form the Azure portal.</span></span>

    <span data-ttu-id="ba6d1-184">e.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-184">e.</span></span> <span data-ttu-id="ba6d1-185">Alternativet för att växla **ON** och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-185">Switch the option to **ON** and click on **SAVE**.</span></span>   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ba6d1-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ba6d1-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="ba6d1-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-187">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ba6d1-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ba6d1-190">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-190">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ba6d1-192">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-192">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ba6d1-194">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-194">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ba6d1-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba6d1-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ba6d1-198">a.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-198">a.</span></span> <span data-ttu-id="ba6d1-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ba6d1-200">b.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-200">b.</span></span> <span data-ttu-id="ba6d1-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ba6d1-202">c.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-202">c.</span></span> <span data-ttu-id="ba6d1-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ba6d1-204">d.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-204">d.</span></span> <span data-ttu-id="ba6d1-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="ba6d1-206">Skapa en testanvändare Teamphoria</span><span class="sxs-lookup"><span data-stu-id="ba6d1-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="ba6d1-207">För att aktivera Azure AD-användare att logga in på Teamphoria etableras de i Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-207">In order to enable Azure AD users to log into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="ba6d1-208">När det gäller Teamphoria är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-208">In the case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="ba6d1-209">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-209">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="ba6d1-210">Logga in på webbplatsen Teamphoria företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-210">Log in to your Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="ba6d1-211">Klicka på **ADMIN** inställningar i verktygsfältet till vänster och under den **hantera** fliken Klicka på **användare** att öppna sidan för användare.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-211">Click on **ADMIN** settings on the left toolbar and under the **MANAGE** tab Click on **USERS** to open the admin page for users.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="ba6d1-213">Klicka på den **manuell bjuda in** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-213">Click on the **MANUAL INVITE** option.</span></span>

    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="ba6d1-215">Utför följande åtgärder på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-215">On this page, perform following action.</span></span> 
    
    ![Bjud in personer](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="ba6d1-217">a.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-217">a.</span></span> <span data-ttu-id="ba6d1-218">I den **e-postadress** textruta den **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-218">In the **EMAIL ADDRESS** textbox, the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ba6d1-219">b.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-219">b.</span></span> <span data-ttu-id="ba6d1-220">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-220">In the **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="ba6d1-221">c.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-221">c.</span></span> <span data-ttu-id="ba6d1-222">I den **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-222">In the **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="ba6d1-223">d.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-223">d.</span></span> <span data-ttu-id="ba6d1-224">Klicka på **inbjudan 1 användaren**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="ba6d1-225">Användare måste acceptera inbjudan att skapas i systemet.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-225">User needs to accept the invite to get created in the system.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ba6d1-226">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ba6d1-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ba6d1-227">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-227">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Teamphoria.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ba6d1-229">**Om du vill tilldela Teamphoria Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ba6d1-229">**To assign Britta Simon to Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="ba6d1-230">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-230">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ba6d1-232">Välj i listan med program **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-232">In the applications list, select **Teamphoria**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="ba6d1-234">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ba6d1-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-236">Click **Add** button.</span></span> <span data-ttu-id="ba6d1-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ba6d1-239">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ba6d1-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ba6d1-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ba6d1-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba6d1-242">Testing single sign-on</span></span>

<span data-ttu-id="ba6d1-243">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ba6d1-244">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ba6d1-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="ba6d1-245">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="ba6d1-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ba6d1-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ba6d1-246">Additional resources</span></span>

* [<span data-ttu-id="ba6d1-247">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ba6d1-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ba6d1-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ba6d1-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

