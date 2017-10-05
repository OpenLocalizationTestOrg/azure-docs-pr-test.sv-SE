---
title: "Självstudier: Azure Active Directory-integrering med Procore SSO | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Procore enkel inloggning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 042a41eaa6bb21670b4c12068f1b017222f64b99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="4347e-103">Självstudier: Azure Active Directory-integrering med Procore enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4347e-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="4347e-104">I kursen får lära du att integrera Procore enkel inloggning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4347e-104">In this tutorial, you learn how to integrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4347e-105">Integrera Procore enkel inloggning med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4347e-105">Integrating Procore SSO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4347e-106">Du kan styra i Azure AD som har åtkomst till Procore SSO</span><span class="sxs-lookup"><span data-stu-id="4347e-106">You can control in Azure AD who has access to Procore SSO</span></span>
- <span data-ttu-id="4347e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Procore SSO (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4347e-107">You can enable your users to automatically get signed-on to Procore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4347e-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="4347e-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="4347e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4347e-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Procore SSO, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="4347e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4347e-110">Prerequisites</span></span>

<span data-ttu-id="4347e-111">För att konfigurera Azure AD-integrering med Procore enkel inloggning, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4347e-111">To configure Azure AD integration with Procore SSO, you need the following items:</span></span>

- <span data-ttu-id="4347e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4347e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4347e-113">En Procore SSO enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4347e-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4347e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4347e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4347e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4347e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4347e-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4347e-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="4347e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4347e-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4347e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4347e-118">Scenario description</span></span>
<span data-ttu-id="4347e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4347e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4347e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4347e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4347e-121">Att lägga till Procore SSO från galleriet</span><span class="sxs-lookup"><span data-stu-id="4347e-121">Adding Procore SSO from the gallery</span></span>
2. <span data-ttu-id="4347e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4347e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-the-gallery"></a><span data-ttu-id="4347e-123">Att lägga till Procore SSO från galleriet</span><span class="sxs-lookup"><span data-stu-id="4347e-123">Adding Procore SSO from the gallery</span></span>
<span data-ttu-id="4347e-124">Du måste lägga till Procore SSO från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Procore SSO i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4347e-124">To configure the integration of Procore SSO into Azure AD, you need to add Procore SSO from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4347e-125">**Utför följande steg för att lägga till Procore SSO från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4347e-125">**To add Procore SSO from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4347e-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4347e-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4347e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4347e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4347e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4347e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4347e-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4347e-133">I sökrutan skriver **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="4347e-133">In the search box, type **Procore SSO**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="4347e-135">Välj i resultatpanelen **Procore SSO**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4347e-135">In the results panel, select **Procore SSO**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4347e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4347e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4347e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Procore SSO baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4347e-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4347e-139">Azure AD måste du känna till motsvarande användaren Procore SSO till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4347e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Procore SSO is to a user in Azure AD.</span></span> <span data-ttu-id="4347e-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren Procore SSO upprättas.</span><span class="sxs-lookup"><span data-stu-id="4347e-140">In other words, a link relationship between an Azure AD user and the related user in Procore SSO needs to be established.</span></span>

<span data-ttu-id="4347e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** Procore sso.</span><span class="sxs-lookup"><span data-stu-id="4347e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Procore SSO.</span></span>

<span data-ttu-id="4347e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Procore enkel inloggning måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4347e-142">To configure and test Azure AD single sign-on with Procore SSO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4347e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4347e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4347e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4347e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4347e-145">**[Skapa en testanvändare Procore SSO](#creating-a-procore-sso-test-user)**  – du har en motsvarighet för Britta Simon Procore SSO som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="4347e-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - to have a counterpart of Britta Simon in Procore SSO that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="4347e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4347e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4347e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4347e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4347e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4347e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4347e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Procore SSO-program.</span><span class="sxs-lookup"><span data-stu-id="4347e-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="4347e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Procore enkel inloggning:**</span><span class="sxs-lookup"><span data-stu-id="4347e-150">**To configure Azure AD single sign-on with Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="4347e-151">I Azure-hanteringsportalen på den **Procore SSO** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4347e-151">In the Azure Management portal, on the **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4347e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="4347e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="4347e-155">På den **Procore domän för SSO och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="4347e-155">On the **Procore SSO Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="4347e-157">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4347e-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="4347e-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4347e-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4347e-161">På den **Procore SSO Configuration** klickar du på **Konfigurera enkel inloggning för Procore** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4347e-161">On the **Procore SSO Configuration** section, click **Configure Procore SSO** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4347e-162">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4347e-162">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="4347e-164">Konfigurera enkel inloggning på **Procore SSO** sida, logga in på webbplatsen procore företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="4347e-164">To configure single sign-on on **Procore SSO** side, login to your procore company site as an administrator.</span></span>

8. <span data-ttu-id="4347e-165">I nedrullningsbara verktygslådan nedåt, klickar du på **Admin** att öppna inställningssidan för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4347e-165">From the toolbox drop down, click on **Admin** to open the SSO settings page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="4347e-167">Klistra in värden i rutorna enligt beskrivningen nedan-</span><span class="sxs-lookup"><span data-stu-id="4347e-167">Paste the values in the boxes as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="4347e-169">a.</span><span class="sxs-lookup"><span data-stu-id="4347e-169">a.</span></span> <span data-ttu-id="4347e-170">I den **enkel inloggning på utfärdar-URL** rutan och klistra in SAML enhets-ID som kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4347e-170">In the **Single Sign On Issuer URL** box, paste the SAML Entity ID copied from the Azure portal.</span></span>

    <span data-ttu-id="4347e-171">b.</span><span class="sxs-lookup"><span data-stu-id="4347e-171">b.</span></span> <span data-ttu-id="4347e-172">I den **SAML-inloggning på mål-URL** rutan och klistra in SAML enkel inloggning tjänstens Webbadress kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4347e-172">In the **SAML Sign On Target URL** box, paste the SAML Single Sign-On Service URL copied from the Azure portal.</span></span>

    <span data-ttu-id="4347e-173">c.</span><span class="sxs-lookup"><span data-stu-id="4347e-173">c.</span></span> <span data-ttu-id="4347e-174">Öppna den **XML-Metadata för** hämtade ovan från Azure-portalen och kopiera certifkatets i taggen med namnet **X509Certificate**.</span><span class="sxs-lookup"><span data-stu-id="4347e-174">Now open the **Metadata XML** downloaded above from the Azure portal and copy the certficate in the tag named **X509Certificate**.</span></span> <span data-ttu-id="4347e-175">Klistra in det kopierade värdet till den **enkel inloggning x509 certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-175">Paste the copied value into the **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="4347e-176">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="4347e-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="4347e-177">När dessa inställningar du måste skicka den **domännamn** (t.ex **contoso.com**) via som du loggar in Procore till den [Procore supportteamet](https://support.procore.com/) och de aktiveras federerad enkel inloggning för domänen.</span><span class="sxs-lookup"><span data-stu-id="4347e-177">After these settings, you needs to send the **domain name** (e.g **contoso.com**) through which you are logging into Procore to the [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4347e-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4347e-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="4347e-179">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4347e-179">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4347e-181">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4347e-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4347e-182">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4347e-182">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4347e-184">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="4347e-184">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4347e-186">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-186">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4347e-188">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4347e-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4347e-190">a.</span><span class="sxs-lookup"><span data-stu-id="4347e-190">a.</span></span> <span data-ttu-id="4347e-191">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4347e-191">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4347e-192">b.</span><span class="sxs-lookup"><span data-stu-id="4347e-192">b.</span></span> <span data-ttu-id="4347e-193">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4347e-193">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4347e-194">c.</span><span class="sxs-lookup"><span data-stu-id="4347e-194">c.</span></span> <span data-ttu-id="4347e-195">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4347e-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4347e-196">d.</span><span class="sxs-lookup"><span data-stu-id="4347e-196">d.</span></span> <span data-ttu-id="4347e-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4347e-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="4347e-198">Skapa en testanvändare Procore SSO</span><span class="sxs-lookup"><span data-stu-id="4347e-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="4347e-199">Följ de nedanstående steg för att skapa en Procore testanvändare på sidan.</span><span class="sxs-lookup"><span data-stu-id="4347e-199">Please follow the below steps to create a Procore test user on their side.</span></span>

1. <span data-ttu-id="4347e-200">Logga in på webbplatsen procore företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="4347e-200">Login to your procore company site as an administrator.</span></span>  

2. <span data-ttu-id="4347e-201">I nedrullningsbara verktygslådan nedåt, klickar du på **Directory** att öppna sidan företagets katalog.</span><span class="sxs-lookup"><span data-stu-id="4347e-201">From the toolbox drop down, click on **Directory** to open the company directory page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="4347e-203">Klicka på **lägga till en Person** alternativet för att öppna formuläret och ange utföra följande alternativ -</span><span class="sxs-lookup"><span data-stu-id="4347e-203">Click on **Add a Person** option to open the form and enter perform following options -</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="4347e-205">a.</span><span class="sxs-lookup"><span data-stu-id="4347e-205">a.</span></span> <span data-ttu-id="4347e-206">I den **Förnamn** textruta typen användarens förnamn som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="4347e-206">In the **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="4347e-207">b.</span><span class="sxs-lookup"><span data-stu-id="4347e-207">b.</span></span> <span data-ttu-id="4347e-208">I den **efternamn** textruta typen användarens efternamn som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="4347e-208">In the **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="4347e-209">c.</span><span class="sxs-lookup"><span data-stu-id="4347e-209">c.</span></span> <span data-ttu-id="4347e-210">I den **e-postadress** textruta typen användarens e-postadress som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="4347e-210">In the **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="4347e-211">d.</span><span class="sxs-lookup"><span data-stu-id="4347e-211">d.</span></span> <span data-ttu-id="4347e-212">Välj **behörighet mallen** som **tillämpa behörighet mall senare**.</span><span class="sxs-lookup"><span data-stu-id="4347e-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="4347e-213">e.</span><span class="sxs-lookup"><span data-stu-id="4347e-213">e.</span></span> <span data-ttu-id="4347e-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4347e-214">Click **Create**.</span></span>

4. <span data-ttu-id="4347e-215">Kontrollera och uppdatera informationen för den nya kontakten.</span><span class="sxs-lookup"><span data-stu-id="4347e-215">Check and update the details for the newly added contact.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="4347e-217">Klicka på **spara och skicka Invitiation** (om det krävs en inbjudan via e-post) eller **spara** (spara direkt) för att slutföra registreringen av användaren.</span><span class="sxs-lookup"><span data-stu-id="4347e-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) to complete the user registration.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4347e-219">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4347e-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4347e-220">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Procore enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4347e-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Procore SSO.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4347e-222">**Om du vill tilldela Procore SSO Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4347e-222">**To assign Britta Simon to Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="4347e-223">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4347e-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4347e-225">Välj i listan med program **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="4347e-225">In the applications list, select **Procore SSO**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="4347e-227">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4347e-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4347e-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4347e-229">Click **Add** button.</span></span> <span data-ttu-id="4347e-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4347e-232">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4347e-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4347e-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4347e-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4347e-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4347e-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4347e-235">Testing single sign-on</span></span>

<span data-ttu-id="4347e-236">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4347e-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4347e-237">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4347e-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="4347e-238">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4347e-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="4347e-239">När du klickar på panelen Procore SSO på åtkomstpanelen du bör få automatiskt loggat in på ditt Procore SSO-program.</span><span class="sxs-lookup"><span data-stu-id="4347e-239">When you click the Procore SSO tile in the Access Panel, you should get automatically signed-on to your Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4347e-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4347e-240">Additional resources</span></span>

* [<span data-ttu-id="4347e-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4347e-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4347e-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4347e-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

