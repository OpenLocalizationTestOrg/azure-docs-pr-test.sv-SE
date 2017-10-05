---
title: "Självstudier: Azure Active Directory-integrering med ScreenSteps | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ScreenSteps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: b6ded8ba1adf03fdccbdb7573c09fae1857c8b16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="6af8e-103">Självstudier: Azure Active Directory-integrering med ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="6af8e-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="6af8e-104">I kursen får lära du att integrera ScreenSteps med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6af8e-104">In this tutorial, you learn how to integrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6af8e-105">Integrera ScreenSteps med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6af8e-105">Integrating ScreenSteps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6af8e-106">Du kan styra i Azure AD som har åtkomst till ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6af8e-106">You can control in Azure AD who has access to ScreenSteps.</span></span>
- <span data-ttu-id="6af8e-107">Du kan aktivera användarna att automatiskt hämta loggat in på ScreenSteps (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="6af8e-107">You can enable your users to automatically get signed-on to ScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6af8e-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6af8e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6af8e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6af8e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6af8e-110">Prerequisites</span></span>

<span data-ttu-id="6af8e-111">För att konfigurera Azure AD-integrering med ScreenSteps, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6af8e-111">To configure Azure AD integration with ScreenSteps, you need the following items:</span></span>

- <span data-ttu-id="6af8e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6af8e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6af8e-113">En ScreenSteps enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6af8e-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6af8e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6af8e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6af8e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6af8e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6af8e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6af8e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6af8e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6af8e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6af8e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6af8e-118">Scenario description</span></span>
<span data-ttu-id="6af8e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6af8e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6af8e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6af8e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6af8e-121">Att lägga till ScreenSteps från galleriet</span><span class="sxs-lookup"><span data-stu-id="6af8e-121">Adding ScreenSteps from the gallery</span></span>
2. <span data-ttu-id="6af8e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6af8e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-the-gallery"></a><span data-ttu-id="6af8e-123">Att lägga till ScreenSteps från galleriet</span><span class="sxs-lookup"><span data-stu-id="6af8e-123">Adding ScreenSteps from the gallery</span></span>
<span data-ttu-id="6af8e-124">Du måste lägga till ScreenSteps från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ScreenSteps i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6af8e-124">To configure the integration of ScreenSteps into Azure AD, you need to add ScreenSteps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6af8e-125">**Utför följande steg för att lägga till ScreenSteps från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="6af8e-125">**To add ScreenSteps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6af8e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6af8e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="6af8e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6af8e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="6af8e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="6af8e-133">I sökrutan skriver **ScreenSteps**väljer **ScreenSteps** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6af8e-133">In the search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button to add the application.</span></span>

    ![ScreenSteps i resultatlistan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6af8e-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6af8e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6af8e-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScreenSteps baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6af8e-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6af8e-137">Azure AD måste du känna till användaren i ScreenSteps motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="6af8e-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ScreenSteps is to a user in Azure AD.</span></span> <span data-ttu-id="6af8e-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ScreenSteps upprättas.</span><span class="sxs-lookup"><span data-stu-id="6af8e-138">In other words, a link relationship between an Azure AD user and the related user in ScreenSteps needs to be established.</span></span>

<span data-ttu-id="6af8e-139">I ScreenSteps, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-139">In ScreenSteps, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6af8e-140">Om du vill konfigurera och testa Azure AD enkel inloggning med ScreenSteps, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6af8e-140">To configure and test Azure AD single sign-on with ScreenSteps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6af8e-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6af8e-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6af8e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6af8e-143">**[Skapa en testanvändare ScreenSteps](#create-a-screensteps-test-user)**  – du har en motsvarighet för Britta Simon i ScreenSteps som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6af8e-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - to have a counterpart of Britta Simon in ScreenSteps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6af8e-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6af8e-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6af8e-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6af8e-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6af8e-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6af8e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6af8e-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ScreenSteps program.</span><span class="sxs-lookup"><span data-stu-id="6af8e-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="6af8e-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ScreenSteps:**</span><span class="sxs-lookup"><span data-stu-id="6af8e-148">**To configure Azure AD single sign-on with ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="6af8e-149">I Azure-portalen på den **ScreenSteps** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-149">In the Azure portal, on the **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="6af8e-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6af8e-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="6af8e-153">På den **ScreenSteps domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6af8e-153">On the **ScreenSteps Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och ScreenSteps domän med enkel inloggning information](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="6af8e-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="6af8e-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6af8e-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6af8e-156">This value is not real.</span></span> <span data-ttu-id="6af8e-157">Uppdatera det här värdet med det faktiska inloggnings-URL, som beskrivs senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-157">Update this value with the actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="6af8e-158">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6af8e-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="6af8e-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6af8e-162">På den **ScreenSteps Configuration** klickar du på **konfigurera ScreenSteps** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6af8e-162">On the **ScreenSteps Configuration** section, click **Configure ScreenSteps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6af8e-163">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6af8e-163">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![ScreenSteps konfiguration](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="6af8e-165">Logga in på webbplatsen ScreenSteps företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="6af8e-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="6af8e-166">Klicka på **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="6af8e-167">![Kontohantering](./media/active-directory-saas-screensteps-tutorial/ic778523.png "kontohantering")</span><span class="sxs-lookup"><span data-stu-id="6af8e-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="6af8e-168">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="6af8e-169">![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span><span class="sxs-lookup"><span data-stu-id="6af8e-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="6af8e-170">Klicka på **skapa slutpunkten för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="6af8e-171">![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span><span class="sxs-lookup"><span data-stu-id="6af8e-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="6af8e-172">I den **skapa enkel inloggning Endpoint** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6af8e-172">In the **Create Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="6af8e-173">![Skapa en slutpunkt för autentisering](./media/active-directory-saas-screensteps-tutorial/ic778526.png "skapa en slutpunkt för autentisering")</span><span class="sxs-lookup"><span data-stu-id="6af8e-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="6af8e-174">a.</span><span class="sxs-lookup"><span data-stu-id="6af8e-174">a.</span></span> <span data-ttu-id="6af8e-175">I den **rubrik** textruta skriver du ett namn.</span><span class="sxs-lookup"><span data-stu-id="6af8e-175">In the **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="6af8e-176">b.</span><span class="sxs-lookup"><span data-stu-id="6af8e-176">b.</span></span> <span data-ttu-id="6af8e-177">Från den **läge** väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-177">From the **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="6af8e-178">c.</span><span class="sxs-lookup"><span data-stu-id="6af8e-178">c.</span></span> <span data-ttu-id="6af8e-179">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-179">Click **Create**.</span></span>

12. <span data-ttu-id="6af8e-180">**Redigera** ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6af8e-180">**Edit** the new endpoint.</span></span>

    <span data-ttu-id="6af8e-181">![Redigera endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "redigera slutpunkt")</span><span class="sxs-lookup"><span data-stu-id="6af8e-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="6af8e-182">I den **Redigera enkel inloggning Endpoint** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6af8e-182">In the **Edit Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="6af8e-183">![Remote autentiseringsslutpunkten](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication slutpunkt")</span><span class="sxs-lookup"><span data-stu-id="6af8e-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="6af8e-184">a.</span><span class="sxs-lookup"><span data-stu-id="6af8e-184">a.</span></span> <span data-ttu-id="6af8e-185">Klicka på **överför ny SAML certifikatfilen**, och sedan ladda upp certifikatet som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-185">Click **Upload new SAML Certificate file**, and then upload the certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="6af8e-186">b.</span><span class="sxs-lookup"><span data-stu-id="6af8e-186">b.</span></span> <span data-ttu-id="6af8e-187">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från Azure-portalen i den **Remote inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="6af8e-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="6af8e-188">c.</span><span class="sxs-lookup"><span data-stu-id="6af8e-188">c.</span></span> <span data-ttu-id="6af8e-189">Klistra in **Sign-Out URL** -värde som du har kopierat från Azure-portalen i den **URL för utloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="6af8e-189">Paste **Sign-Out URL** value, which you have copied from the Azure portal into the **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="6af8e-190">d.</span><span class="sxs-lookup"><span data-stu-id="6af8e-190">d.</span></span> <span data-ttu-id="6af8e-191">Välj en **grupp** att tilldela användare till när de har etablerats.</span><span class="sxs-lookup"><span data-stu-id="6af8e-191">Select a **Group** to assign users to when they are provisioned.</span></span>
    
    <span data-ttu-id="6af8e-192">e.</span><span class="sxs-lookup"><span data-stu-id="6af8e-192">e.</span></span> <span data-ttu-id="6af8e-193">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-193">Click **Update**.</span></span>

    <span data-ttu-id="6af8e-194">f.</span><span class="sxs-lookup"><span data-stu-id="6af8e-194">f.</span></span> <span data-ttu-id="6af8e-195">Kopiera den **konsument-URL för SAML** till Urklipp och klistra in till den **inloggnings-URL** TextBox-kontroll i **ScreenSteps domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6af8e-195">Copy the **SAML Consumer URL** to the clipboard and paste in to the **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="6af8e-196">g.</span><span class="sxs-lookup"><span data-stu-id="6af8e-196">g.</span></span> <span data-ttu-id="6af8e-197">Gå tillbaka till den **redigera slutpunkten för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-197">Return to the **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="6af8e-198">h.</span><span class="sxs-lookup"><span data-stu-id="6af8e-198">h.</span></span> <span data-ttu-id="6af8e-199">Klicka på den **göra standard för kontot** om du vill använda den här slutpunkten för alla användare som loggar in i ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6af8e-199">Click the **Make default for account** button to use this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="6af8e-200">Du kan också klicka på den **lägga till webbplatsen** om du vill använda den här slutpunkten för specifika platser i **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-200">Alternatively you can click the **Add to Site** button to use this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="6af8e-201">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="6af8e-201">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6af8e-202">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="6af8e-202">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6af8e-203">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6af8e-203">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6af8e-204">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6af8e-204">Create an Azure AD test user</span></span>

<span data-ttu-id="6af8e-205">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6af8e-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="6af8e-207">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="6af8e-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6af8e-208">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-208">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6af8e-210">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-210">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6af8e-212">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-212">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6af8e-214">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6af8e-214">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6af8e-216">a.</span><span class="sxs-lookup"><span data-stu-id="6af8e-216">a.</span></span> <span data-ttu-id="6af8e-217">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-217">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6af8e-218">b.</span><span class="sxs-lookup"><span data-stu-id="6af8e-218">b.</span></span> <span data-ttu-id="6af8e-219">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6af8e-219">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6af8e-220">c.</span><span class="sxs-lookup"><span data-stu-id="6af8e-220">c.</span></span> <span data-ttu-id="6af8e-221">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-221">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6af8e-222">d.</span><span class="sxs-lookup"><span data-stu-id="6af8e-222">d.</span></span> <span data-ttu-id="6af8e-223">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="6af8e-224">Skapa en testanvändare ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="6af8e-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="6af8e-225">I det här avsnittet skapar du en användare som kallas Britta Simon i ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6af8e-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="6af8e-226">Arbeta med [ScreenSteps klienten supportteamet](https://www.screensteps.com/contact) att lägga till användare i ScreenSteps-plattformen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add the users in the ScreenSteps platform.</span></span> <span data-ttu-id="6af8e-227">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6af8e-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6af8e-228">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6af8e-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="6af8e-229">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="6af8e-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ScreenSteps.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="6af8e-231">**Om du vill tilldela ScreenSteps Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6af8e-231">**To assign Britta Simon to ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="6af8e-232">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6af8e-234">Välj i listan med program **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-234">In the applications list, select **ScreenSteps**.</span></span>

    ![Länken ScreenSteps i listan med program](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="6af8e-236">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6af8e-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="6af8e-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6af8e-238">Click **Add** button.</span></span> <span data-ttu-id="6af8e-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="6af8e-241">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="6af8e-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6af8e-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6af8e-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6af8e-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6af8e-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6af8e-244">Test single sign-on</span></span>

<span data-ttu-id="6af8e-245">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6af8e-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6af8e-246">När du klickar på panelen ScreenSteps på åtkomstpanelen du bör få automatiskt loggat in på ditt ScreenSteps program.</span><span class="sxs-lookup"><span data-stu-id="6af8e-246">When you click the ScreenSteps tile in the Access Panel, you should get automatically signed-on to your ScreenSteps application.</span></span>
<span data-ttu-id="6af8e-247">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6af8e-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6af8e-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6af8e-248">Additional resources</span></span>

* [<span data-ttu-id="6af8e-249">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6af8e-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6af8e-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6af8e-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

