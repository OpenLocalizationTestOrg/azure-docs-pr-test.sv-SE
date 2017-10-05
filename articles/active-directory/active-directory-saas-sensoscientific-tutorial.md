---
title: "Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SensoScientific trådlösa temperatur övervakningssystem."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: fa6242cf7f9559ca394ffde2e5e734cb935b03dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="d2345-103">Självstudier: Azure Active Directory-integrering med SensoScientific trådlösa temperatur övervakningssystem</span><span class="sxs-lookup"><span data-stu-id="d2345-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="d2345-104">I kursen får lära du att integrera SensoScientific trådlösa temperatur övervakningssystem med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d2345-104">In this tutorial, you learn how to integrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d2345-105">Integrera SensoScientific trådlösa temperatur övervakningssystem med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d2345-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d2345-106">Du kan styra i Azure AD som har åtkomst till SensoScientific trådlösa temperatur övervakningssystem</span><span class="sxs-lookup"><span data-stu-id="d2345-106">You can control in Azure AD who has access to SensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="d2345-107">Du kan aktivera användarna att automatiskt hämta loggat in på SensoScientific trådlösa temperatur övervakningssystem (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d2345-107">You can enable your users to automatically get signed-on to SensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d2345-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2345-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d2345-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d2345-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2345-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d2345-110">Prerequisites</span></span>

<span data-ttu-id="d2345-111">För att konfigurera Azure AD-integrering med SensoScientific trådlösa temperatur övervakningssystem, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d2345-111">To configure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need the following items:</span></span>

- <span data-ttu-id="d2345-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d2345-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d2345-113">En SensoScientific trådlösa temperatur övervakningssystem enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d2345-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d2345-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d2345-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d2345-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d2345-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d2345-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d2345-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d2345-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d2345-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d2345-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d2345-118">Scenario description</span></span>
<span data-ttu-id="d2345-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d2345-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d2345-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d2345-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d2345-121">Att lägga till SensoScientific trådlösa temperatur övervakningssystem från galleriet</span><span class="sxs-lookup"><span data-stu-id="d2345-121">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
2. <span data-ttu-id="d2345-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d2345-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a><span data-ttu-id="d2345-123">Att lägga till SensoScientific trådlösa temperatur övervakningssystem från galleriet</span><span class="sxs-lookup"><span data-stu-id="d2345-123">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
<span data-ttu-id="d2345-124">Du måste lägga till SensoScientific trådlösa temperatur övervakningssystem från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SensoScientific trådlösa temperatur övervakningssystem i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2345-124">To configure the integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need to add SensoScientific Wireless Temperature Monitoring System from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d2345-125">**Utför följande steg för att lägga till SensoScientific trådlösa temperatur övervakningssystem från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d2345-125">**To add SensoScientific Wireless Temperature Monitoring System from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d2345-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d2345-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d2345-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d2345-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d2345-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d2345-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d2345-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d2345-133">I sökrutan skriver **SensoScientific trådlösa temperatur övervakningssystem**.</span><span class="sxs-lookup"><span data-stu-id="d2345-133">In the search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="d2345-135">Välj i resultatpanelen **SensoScientific trådlösa temperatur övervakningssystem**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d2345-135">In the results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d2345-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d2345-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d2345-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d2345-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d2345-139">Azure AD måste du känna till motsvarande användaren i SensoScientific trådlösa temperatur övervakningssystem till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d2345-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SensoScientific Wireless Temperature Monitoring System is to a user in Azure AD.</span></span> <span data-ttu-id="d2345-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SensoScientific trådlösa temperatur övervakningssystem upprättas.</span><span class="sxs-lookup"><span data-stu-id="d2345-140">In other words, a link relationship between an Azure AD user and the related user in SensoScientific Wireless Temperature Monitoring System needs to be established.</span></span>

<span data-ttu-id="d2345-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i SensoScientific trådlösa temperatur övervakningssystem.</span><span class="sxs-lookup"><span data-stu-id="d2345-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="d2345-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d2345-142">To configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d2345-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d2345-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d2345-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d2345-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d2345-145">**[Skapa en testanvändare SensoScientific trådlösa temperatur övervakningssystem](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  – har en motsvarighet för Britta Simon SensoScientific trådlösa temperatur övervakningssystem som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d2345-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - to have a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d2345-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d2345-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d2345-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d2345-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d2345-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d2345-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d2345-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt övervakningssystem för SensoScientific trådlösa temperatur-program.</span><span class="sxs-lookup"><span data-stu-id="d2345-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="d2345-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SensoScientific trådlösa temperatur övervakningssystem:**</span><span class="sxs-lookup"><span data-stu-id="d2345-150">**To configure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="d2345-151">I Azure-portalen på den **SensoScientific trådlösa temperatur övervakningssystem** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d2345-151">In the Azure portal, on the **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d2345-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d2345-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="d2345-155">På den **SensoScientific trådlösa temperatur övervakning domän och URL: er** avsnitt, behöver du inte utföra några steg som appen är redan förintegrerade med Azure:</span><span class="sxs-lookup"><span data-stu-id="d2345-155">On the **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need to perform any steps as the app is already pre-integrated with Azure:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="d2345-157">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d2345-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="d2345-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d2345-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d2345-161">På den **övervakning systemkonfigurationen för SensoScientific trådlösa temperatur** klickar du på **konfigurera SensoScientific trådlösa temperatur övervakningssystem** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d2345-161">On the **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d2345-162">Kopiera den **Sign-Out URL, SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d2345-162">Copy the **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="d2345-164">Logga in på ditt övervakningssystem för SensoScientific trådlösa temperatur program som administratör.</span><span class="sxs-lookup"><span data-stu-id="d2345-164">Sign on to your SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="d2345-165">Klicka i navigeringsmenyn upp **Configuration** och gå till **konfigurera** under **enkel inloggning** att öppna enkel inloggning på inställningarna för.</span><span class="sxs-lookup"><span data-stu-id="d2345-165">In the navigation menu on the top, click **Configuration** and goto **Configure** under **Single Sign On** to open the Single Sign On Settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="d2345-167">I **enkel inloggning på inställningar** formuläret utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d2345-167">In **Single Sign On Settings** form perform the following steps:</span></span>
 
    <span data-ttu-id="d2345-168">a.</span><span class="sxs-lookup"><span data-stu-id="d2345-168">a.</span></span> <span data-ttu-id="d2345-169">Välj **utfärdarnamnet** som Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2345-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="d2345-170">b.</span><span class="sxs-lookup"><span data-stu-id="d2345-170">b.</span></span> <span data-ttu-id="d2345-171">Klistra in den **SAML enhets-ID** som du har kopierat från Azure-portalen i utfärdar-URL-textrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-171">Paste the **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="d2345-172">c.</span><span class="sxs-lookup"><span data-stu-id="d2345-172">c.</span></span> <span data-ttu-id="d2345-173">Klistra in den **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i textrutan för enkel inloggning tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="d2345-173">Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="d2345-174">d.</span><span class="sxs-lookup"><span data-stu-id="d2345-174">d.</span></span> <span data-ttu-id="d2345-175">Klistra in den **Sign-Out URL** som du har kopierat från Azure-portalen i tjänst-URL för enkel Sign-Out textruta.</span><span class="sxs-lookup"><span data-stu-id="d2345-175">Paste the **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="d2345-176">e.</span><span class="sxs-lookup"><span data-stu-id="d2345-176">e.</span></span> <span data-ttu-id="d2345-177">Bläddra certifikat som du har hämtat från Azure-portalen och överföra här.</span><span class="sxs-lookup"><span data-stu-id="d2345-177">Browse the certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="d2345-178">f.</span><span class="sxs-lookup"><span data-stu-id="d2345-178">f.</span></span> <span data-ttu-id="d2345-179">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d2345-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="d2345-180">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d2345-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d2345-181">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d2345-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d2345-182">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d2345-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d2345-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d2345-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="d2345-184">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d2345-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d2345-186">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d2345-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d2345-187">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d2345-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d2345-189">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d2345-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d2345-191">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d2345-193">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d2345-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d2345-195">a.</span><span class="sxs-lookup"><span data-stu-id="d2345-195">a.</span></span> <span data-ttu-id="d2345-196">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d2345-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d2345-197">b.</span><span class="sxs-lookup"><span data-stu-id="d2345-197">b.</span></span> <span data-ttu-id="d2345-198">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d2345-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d2345-199">c.</span><span class="sxs-lookup"><span data-stu-id="d2345-199">c.</span></span> <span data-ttu-id="d2345-200">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d2345-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d2345-201">d.</span><span class="sxs-lookup"><span data-stu-id="d2345-201">d.</span></span> <span data-ttu-id="d2345-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2345-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="d2345-203">Skapa en SensoScientific trådlösa temperatur övervakningssystem testanvändare</span><span class="sxs-lookup"><span data-stu-id="d2345-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="d2345-204">Om du vill aktivera Azure AD-användare kan logga in på SensoScientific trådlösa temperatur övervakningssystem etableras de till SensoScientific trådlösa temperatur övervakningssystem.</span><span class="sxs-lookup"><span data-stu-id="d2345-204">To enable Azure AD users to log in to SensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="d2345-205">Arbeta med [SensoScientific trådlösa temperatur övervakningssystem supportteamet](https://www.sensoscientific.com/contact-us/) att lägga till användare i SensoScientific trådlösa temperatur övervakningssystem-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d2345-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add the users in the SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="d2345-206">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d2345-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d2345-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d2345-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d2345-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SensoScientific trådlösa temperatur övervakningssystem.</span><span class="sxs-lookup"><span data-stu-id="d2345-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SensoScientific Wireless Temperature Monitoring System.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d2345-210">**Om du vill tilldela SensoScientific trådlösa temperatur övervakningssystem Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d2345-210">**To assign Britta Simon to SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="d2345-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d2345-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d2345-213">Välj i listan med program **SensoScientific trådlösa temperatur övervakningssystem**.</span><span class="sxs-lookup"><span data-stu-id="d2345-213">In the applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="d2345-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d2345-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d2345-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d2345-217">Click **Add** button.</span></span> <span data-ttu-id="d2345-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d2345-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d2345-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d2345-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d2345-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d2345-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d2345-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d2345-223">Testing single sign-on</span></span>

<span data-ttu-id="d2345-224">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d2345-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="d2345-225">Klicka på panelen övervakningssystem för SensoScientific trådlösa temperatur på åtkomstpanelen, du kommer att automatiskt loggat in på ditt övervakningssystem för SensoScientific trådlösa temperatur program.</span><span class="sxs-lookup"><span data-stu-id="d2345-225">Click the SensoScientific Wireless Temperature Monitoring System tile in the Access Panel, you will be automatically signed-on to your SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="d2345-226">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="d2345-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2345-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2345-227">Additional resources</span></span>

* [<span data-ttu-id="d2345-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2345-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d2345-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d2345-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

