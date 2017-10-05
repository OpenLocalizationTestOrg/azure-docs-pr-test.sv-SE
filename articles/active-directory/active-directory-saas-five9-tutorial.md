---
title: "Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter) | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Five9 Plus-kort (CTI, kontakta Center-agenter)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: d75163ea5eb3fa811e07861f06e6c4d5c758b898
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a><span data-ttu-id="b621e-103">Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b621e-103">Tutorial: Azure Active Directory integration with Five9 Plus Adapter (CTI, Contact Center Agents)</span></span>

<span data-ttu-id="b621e-104">I kursen får du lära dig hur du integrerar Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b621e-104">In this tutorial, you learn how to integrate Five9 Plus Adapter (CTI, Contact Center Agents) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b621e-105">Integrera Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b621e-105">Integrating Five9 Plus Adapter (CTI, Contact Center Agents) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b621e-106">Du kan styra i Azure AD som har åtkomst till Five9 Plus nätverkskort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b621e-106">You can control in Azure AD who has access to Five9 Plus Adapter (CTI, Contact Center Agents)</span></span>
- <span data-ttu-id="b621e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Five9 Plus nätverkskort (CTI, kontakta Center-agenter) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b621e-107">You can enable your users to automatically get signed-on to Five9 Plus Adapter (CTI, Contact Center Agents) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b621e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b621e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b621e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b621e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b621e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b621e-110">Prerequisites</span></span>

<span data-ttu-id="b621e-111">Om du vill konfigurera Azure AD-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter) behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b621e-111">To configure Azure AD integration with Five9 Plus Adapter (CTI, Contact Center Agents), you need the following items:</span></span>

- <span data-ttu-id="b621e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b621e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b621e-113">En Five9 Plus nätverkskort (CTI, kontakta Center-agenter) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b621e-113">A Five9 Plus Adapter (CTI, Contact Center Agents) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b621e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b621e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b621e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b621e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b621e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b621e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b621e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b621e-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b621e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b621e-118">Scenario description</span></span>
<span data-ttu-id="b621e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b621e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b621e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b621e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b621e-121">Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från galleriet</span><span class="sxs-lookup"><span data-stu-id="b621e-121">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from the gallery</span></span>
2. <span data-ttu-id="b621e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b621e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a><span data-ttu-id="b621e-123">Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från galleriet</span><span class="sxs-lookup"><span data-stu-id="b621e-123">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from the gallery</span></span>
<span data-ttu-id="b621e-124">För att konfigurera integrering av Five9 Plus nätverkskort (CTI, kontakta Center-agenter) till Azure AD, måste du lägga till Five9 Plus nätverkskort (CTI, kontakta Center-agenter) från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b621e-124">To configure the integration of Five9 Plus Adapter (CTI, Contact Center Agents) into Azure AD, you need to add Five9 Plus Adapter (CTI, Contact Center Agents) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b621e-125">**Utför följande steg om du vill lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b621e-125">**To add Five9 Plus Adapter (CTI, Contact Center Agents) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b621e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b621e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b621e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b621e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b621e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b621e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b621e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b621e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b621e-133">I sökrutan skriver **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.</span><span class="sxs-lookup"><span data-stu-id="b621e-133">In the search box, type **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. <span data-ttu-id="b621e-135">Välj i resultatpanelen **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b621e-135">In the results panel, select **Five9 Plus Adapter (CTI, Contact Center Agents)**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b621e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b621e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b621e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b621e-138">In this section, you configure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b621e-139">Azure AD måste du känna till motsvarande användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b621e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Five9 Plus Adapter (CTI, Contact Center Agents) is to a user in Azure AD.</span></span> <span data-ttu-id="b621e-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) upprättas.</span><span class="sxs-lookup"><span data-stu-id="b621e-140">In other words, a link relationship between an Azure AD user and the related user in Five9 Plus Adapter (CTI, Contact Center Agents) needs to be established.</span></span>

<span data-ttu-id="b621e-141">I Five9 Plus-kort (CTI, kontakta Center-agenter), tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b621e-141">In Five9 Plus Adapter (CTI, Contact Center Agents), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b621e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter), måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b621e-142">To configure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b621e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b621e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b621e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b621e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b621e-145">**[Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  – du har en motsvarighet för Britta Simon i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b621e-145">**[Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - to have a counterpart of Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b621e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b621e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b621e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b621e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b621e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b621e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b621e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b621e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>

<span data-ttu-id="b621e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter):**</span><span class="sxs-lookup"><span data-stu-id="b621e-150">**To configure Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), perform the following steps:**</span></span>

1. <span data-ttu-id="b621e-151">I Azure-portalen på den **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b621e-151">In the Azure portal, on the **Five9 Plus Adapter (CTI, Contact Center Agents)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b621e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b621e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. <span data-ttu-id="b621e-155">På den **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b621e-155">On the **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    <span data-ttu-id="b621e-157">a.</span><span class="sxs-lookup"><span data-stu-id="b621e-157">a.</span></span> <span data-ttu-id="b621e-158">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="b621e-158">In the **Identifier** textbox, type a URL using the following patterns:</span></span>

    |    <span data-ttu-id="b621e-159">Miljö</span><span class="sxs-lookup"><span data-stu-id="b621e-159">Environment</span></span>      |       <span data-ttu-id="b621e-160">URL: EN</span><span class="sxs-lookup"><span data-stu-id="b621e-160">URL</span></span>      |
    | :-- | :-- |
    | <span data-ttu-id="b621e-161">För ”Five9 Plus Adapter för Microsoft Dynamics CRM”</span><span class="sxs-lookup"><span data-stu-id="b621e-161">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | <span data-ttu-id="b621e-162">För ”Five9 Plus kortet för Zendesk”</span><span class="sxs-lookup"><span data-stu-id="b621e-162">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | <span data-ttu-id="b621e-163">För ”Five9 Plus kortet för agenten skrivbord Toolkit”</span><span class="sxs-lookup"><span data-stu-id="b621e-163">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    <span data-ttu-id="b621e-164">b.</span><span class="sxs-lookup"><span data-stu-id="b621e-164">b.</span></span> <span data-ttu-id="b621e-165">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="b621e-165">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>

    |      <span data-ttu-id="b621e-166">Miljö</span><span class="sxs-lookup"><span data-stu-id="b621e-166">Environment</span></span>     |      <span data-ttu-id="b621e-167">URL: EN</span><span class="sxs-lookup"><span data-stu-id="b621e-167">URL</span></span>      |
    | :--                  | :--           |
    | <span data-ttu-id="b621e-168">För ”Five9 Plus Adapter för Microsoft Dynamics CRM”</span><span class="sxs-lookup"><span data-stu-id="b621e-168">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | <span data-ttu-id="b621e-169">För ”Five9 Plus kortet för Zendesk”</span><span class="sxs-lookup"><span data-stu-id="b621e-169">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | <span data-ttu-id="b621e-170">För ”Five9 Plus kortet för agenten skrivbord Toolkit”</span><span class="sxs-lookup"><span data-stu-id="b621e-170">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. <span data-ttu-id="b621e-171">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b621e-171">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. <span data-ttu-id="b621e-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b621e-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b621e-175">På den **Five9 Plus (CTI, kontakta Center-agenter) kortkonfiguration** klickar du på **konfigurera Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** att öppna **konfigurera inloggning**fönster.</span><span class="sxs-lookup"><span data-stu-id="b621e-175">On the **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** section, click **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b621e-176">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b621e-176">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. <span data-ttu-id="b621e-178">Konfigurera enkel inloggning på **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** sida, måste du skicka den hämtade **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**till [Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact).</span><span class="sxs-lookup"><span data-stu-id="b621e-178">To configure single sign-on on **Five9 Plus Adapter (CTI, Contact Center Agents)** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact).</span></span> <span data-ttu-id="b621e-179">Dessutom, för att konfigurera enkel inloggning ytterligare följer du de nedanstående steg enligt nätverkskortet:</span><span class="sxs-lookup"><span data-stu-id="b621e-179">Also additionally, for configuring SSO further please follow the below steps according to the adapter:</span></span>

    <span data-ttu-id="b621e-180">a.</span><span class="sxs-lookup"><span data-stu-id="b621e-180">a.</span></span> <span data-ttu-id="b621e-181">”Five9 Plus kortet för agenten skrivbord Toolkit” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b621e-181">“Five9 Plus Adapter for Agent Desktop Toolkit” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="b621e-182">b.</span><span class="sxs-lookup"><span data-stu-id="b621e-182">b.</span></span> <span data-ttu-id="b621e-183">”Five9 Plus Adapter för Microsoft Dynamics CRM” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b621e-183">“Five9 Plus Adapter for Microsoft Dynamics CRM” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="b621e-184">c.</span><span class="sxs-lookup"><span data-stu-id="b621e-184">c.</span></span> <span data-ttu-id="b621e-185">”Five9 Plus kortet för Zendesk” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b621e-185">“Five9 Plus Adapter for Zendesk” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span></span>


> [!TIP]
> <span data-ttu-id="b621e-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b621e-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b621e-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b621e-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b621e-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b621e-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b621e-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b621e-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="b621e-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b621e-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b621e-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b621e-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b621e-193">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b621e-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b621e-195">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b621e-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b621e-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b621e-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b621e-199">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b621e-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b621e-201">a.</span><span class="sxs-lookup"><span data-stu-id="b621e-201">a.</span></span> <span data-ttu-id="b621e-202">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b621e-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b621e-203">b.</span><span class="sxs-lookup"><span data-stu-id="b621e-203">b.</span></span> <span data-ttu-id="b621e-204">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b621e-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b621e-205">c.</span><span class="sxs-lookup"><span data-stu-id="b621e-205">c.</span></span> <span data-ttu-id="b621e-206">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b621e-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b621e-207">d.</span><span class="sxs-lookup"><span data-stu-id="b621e-207">d.</span></span> <span data-ttu-id="b621e-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b621e-208">Click **Create**.</span></span>
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a><span data-ttu-id="b621e-209">Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b621e-209">Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user</span></span>

<span data-ttu-id="b621e-210">I det här avsnittet skapar du en användare som kallas Britta Simon i Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b621e-210">In this section, you create a user called Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents).</span></span> <span data-ttu-id="b621e-211">Arbeta med [Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact) att lägga till användare i Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b621e-211">Work with [Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact) to add the users in the Five9 Plus Adapter (CTI, Contact Center Agents) platform.</span></span> <span data-ttu-id="b621e-212">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b621e-212">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b621e-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b621e-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b621e-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b621e-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Five9 Plus Adapter (CTI, Contact Center Agents).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b621e-216">**Om du vill tilldela Britta Simon Five9 Plus nätverkskortet (CTI, kontakta Center-agenter), utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b621e-216">**To assign Britta Simon to Five9 Plus Adapter (CTI, Contact Center Agents), perform the following steps:**</span></span>

1. <span data-ttu-id="b621e-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b621e-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b621e-219">Välj i listan med program **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.</span><span class="sxs-lookup"><span data-stu-id="b621e-219">In the applications list, select **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. <span data-ttu-id="b621e-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b621e-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b621e-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b621e-223">Click **Add** button.</span></span> <span data-ttu-id="b621e-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b621e-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b621e-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b621e-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b621e-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b621e-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b621e-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b621e-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b621e-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b621e-229">Testing single sign-on</span></span>

<span data-ttu-id="b621e-230">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b621e-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b621e-231">När du klickar på panelen Five9 Plus nätverkskort (CTI, kontakta Center-agenter) på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b621e-231">When you click the Five9 Plus Adapter (CTI, Contact Center Agents) tile in the Access Panel, you should get automatically signed-on to your Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>
<span data-ttu-id="b621e-232">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b621e-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b621e-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b621e-233">Additional resources</span></span>

* [<span data-ttu-id="b621e-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b621e-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b621e-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b621e-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

