---
title: aaaHow toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory | Microsoft Docs
description: "Lär dig hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 846c7b3497c8842947c5b406f4362b9e06785b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-saml-based-single-sign-on-tooapplications-in-azure-active-directory"></a><span data-ttu-id="e59a9-103">Hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e59a9-103">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>
<span data-ttu-id="e59a9-104">När du felsöker en SAML-baserade program-integrering är ofta användbara toouse ett verktyg som [Fiddler](http://www.telerik.com/fiddler) toosee hello SAML begäran hello SAML-svar och hello faktiska SAML-token som utfärdas toohello program.</span><span class="sxs-lookup"><span data-stu-id="e59a9-104">When debugging a SAML-based application integration, it is often helpful toouse a tool like [Fiddler](http://www.telerik.com/fiddler) toosee hello SAML request, hello SAML response, and hello actual SAML token that is issued toohello application.</span></span> <span data-ttu-id="e59a9-105">Genom att undersöka hello SAML-token, se till att alla hello begärda attribut, hello användarnamn i hello SAML ämne och hello utfärdaren URI kommer via som förväntat.</span><span class="sxs-lookup"><span data-stu-id="e59a9-105">By examining hello SAML token, you can ensure that all of hello required attributes, hello username in hello SAML subject, and hello issuer URI are coming through as expected.</span></span>

![][1]

<span data-ttu-id="e59a9-106">hello svaret från Azure AD som innehåller hello SAML-token är vanligtvis hello som inträffar efter att en HTTP 302-omdirigering från https://login.windows.net och konfigureras skickade toohello **Reply URL** av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="e59a9-106">hello response from Azure AD that contains hello SAML token is typically hello one that occurs after an HTTP 302 redirect from https://login.windows.net, and is sent toohello configured **Reply URL** of hello application.</span></span> 

<span data-ttu-id="e59a9-107">Du kan visa hello SAML-token genom att välja den här raden och sedan välja hello **kontrollanter > WebForms** fliken i hello högra panelen.</span><span class="sxs-lookup"><span data-stu-id="e59a9-107">You can view hello SAML token by selecting this line and then selecting hello **Inspectors > WebForms** tab in hello right panel.</span></span> <span data-ttu-id="e59a9-108">Högerklicka på hello därifrån **SAMLResponse** värdet och välj **skicka tooTextWizard**.</span><span class="sxs-lookup"><span data-stu-id="e59a9-108">From there, right-click hello **SAMLResponse** value and select **Send tooTextWizard**.</span></span> <span data-ttu-id="e59a9-109">Välj sedan **från Base64** från hello **transformera** menyn toodecode hello token och se innehållet.</span><span class="sxs-lookup"><span data-stu-id="e59a9-109">Then select **From Base64** from hello **Transform** menu toodecode hello token and see its contents.</span></span>

<span data-ttu-id="e59a9-110">**Obs**: toosee hello innehållet i den här HTTP-begäran, Fiddler kan medföra att du tooconfigure dekryptering av HTTPS-trafik som du behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="e59a9-110">**Note**: toosee hello contents of this HTTP request, Fiddler may prompt you tooconfigure decryption of HTTPS traffic, which you will need toodo.</span></span>

## <a name="related-articles"></a><span data-ttu-id="e59a9-111">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="e59a9-111">Related Articles</span></span>
* [<span data-ttu-id="e59a9-112">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e59a9-112">Article Index for Application Management in Azure Active Directory</span></span>](../active-directory-apps-index.md)
* [<span data-ttu-id="e59a9-113">Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="e59a9-113">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](../active-directory-saas-custom-apps.md)
* [<span data-ttu-id="e59a9-114">Hur tooCustomize anspråk som utfärdats i hello SAML-Token för Pre-Integrated appar</span><span class="sxs-lookup"><span data-stu-id="e59a9-114">How tooCustomize Claims Issued in hello SAML Token for Pre-Integrated Apps</span></span>](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png