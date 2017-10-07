---
title: aaaPage mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur toocustomize hello innehållet i developer portalens sidor med en uppsättning mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 84bd971ad4bcacfdd36c2ebbe05b16063f2a547b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="0bdae-103">Mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0bdae-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="0bdae-104">Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="0bdae-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="0bdae-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="0bdae-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="0bdae-106">hello mallar i det här avsnittet kan du toocustomize hello innehållet i hello inloggning, logga in och att hitta inte sidan sidor i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0bdae-106">hello templates in this section allow you toocustomize hello content of hello sign in, sign up, and page not found pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="0bdae-107">Logga in</span><span class="sxs-lookup"><span data-stu-id="0bdae-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="0bdae-108">Registrera sig</span><span class="sxs-lookup"><span data-stu-id="0bdae-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="0bdae-109">Det gick inte att hitta sidan</span><span class="sxs-lookup"><span data-stu-id="0bdae-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="0bdae-110">Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar.</span><span class="sxs-lookup"><span data-stu-id="0bdae-110">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="0bdae-111">Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="0bdae-111">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="0bdae-112">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="0bdae-112">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="0bdae-113"><a name="SignIn"></a>Logga in</span><span class="sxs-lookup"><span data-stu-id="0bdae-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="0bdae-114">Hej **inloggning** mall kan du toocustomize hello logga på sidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0bdae-114">hello **sign in** template allows you toocustomize hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0bdae-115">![Inloggningssidan i](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM inloggning sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="0bdae-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0bdae-116">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0bdae-116">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="0bdae-117">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0bdae-117">Controls</span></span>  
 <span data-ttu-id="0bdae-118">Den här mallen kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0bdae-118">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="0bdae-119">Basic-inloggning</span><span class="sxs-lookup"><span data-stu-id="0bdae-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="0bdae-120">providers</span><span class="sxs-lookup"><span data-stu-id="0bdae-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="0bdae-121">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0bdae-121">Data model</span></span>  
 <span data-ttu-id="0bdae-122">[Användarens](api-management-template-data-model-reference.md#UseSignIn) entitet.</span><span class="sxs-lookup"><span data-stu-id="0bdae-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="0bdae-123">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0bdae-123">Sample template data</span></span>  
  
```json  
{  
    "Email": null,  
    "Password": null,  
    "ReturnUrl": null,  
    "RememberMe": false,  
    "RegistrationEnabled": true,  
    "DelegationEnabled": false,  
    "DelegationUrl": null,  
    "SsoSignUpUrl": null,  
    "AuxServiceUrl": "https://manage.windowsazure.com/#Workspaces/ApiManagementExtension/service/contoso5/dashboard",  
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
    ],  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsEnabled": false  
}  
```  
  
##  <span data-ttu-id="0bdae-124"><a name="SignUp"></a>Registrera sig</span><span class="sxs-lookup"><span data-stu-id="0bdae-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="0bdae-125">Hej **registrering** mall kan du toocustomize hello-registrering i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0bdae-125">hello **sign up** template allows you toocustomize hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0bdae-126">![Inloggningssidan](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM logga in sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="0bdae-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0bdae-127">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0bdae-127">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="0bdae-128">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0bdae-128">Controls</span></span>  
 <span data-ttu-id="0bdae-129">Den här mallen kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0bdae-129">This template may  use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="0bdae-130">registrering</span><span class="sxs-lookup"><span data-stu-id="0bdae-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="0bdae-131">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0bdae-131">Data model</span></span>  
 <span data-ttu-id="0bdae-132">[Användaren loggar in](api-management-template-data-model-reference.md#UserSignUp) entitet.</span><span class="sxs-lookup"><span data-stu-id="0bdae-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="0bdae-133">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0bdae-133">Sample template data</span></span>  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <span data-ttu-id="0bdae-134"><a name="PageNotFound"></a>Det gick inte att hitta sidan</span><span class="sxs-lookup"><span data-stu-id="0bdae-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="0bdae-135">Hej **sidan inte att hitta** mallen kan du toocustomize hello sidan inte att hitta sidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0bdae-135">hello **page not found** template allows you toocustomize hello page not found page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0bdae-136">![Gick inte att hitta sidan](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM gick inte att hitta sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="0bdae-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0bdae-137">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0bdae-137">Default template</span></span>  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a><span data-ttu-id="0bdae-138">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0bdae-138">Controls</span></span>  
 <span data-ttu-id="0bdae-139">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0bdae-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="0bdae-140">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0bdae-140">Data model</span></span>  
  
|<span data-ttu-id="0bdae-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0bdae-141">Property</span></span>|<span data-ttu-id="0bdae-142">Typ</span><span class="sxs-lookup"><span data-stu-id="0bdae-142">Type</span></span>|<span data-ttu-id="0bdae-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0bdae-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="0bdae-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="0bdae-144">referenceCode</span></span>|<span data-ttu-id="0bdae-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="0bdae-145">string</span></span>|<span data-ttu-id="0bdae-146">Kod som genereras om den här sidan visades hello följd av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="0bdae-146">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="0bdae-147">Felkod</span><span class="sxs-lookup"><span data-stu-id="0bdae-147">errorCode</span></span>|<span data-ttu-id="0bdae-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="0bdae-148">string</span></span>|<span data-ttu-id="0bdae-149">Kod som genereras om den här sidan visades hello följd av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="0bdae-149">Code generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="0bdae-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="0bdae-150">emailBody</span></span>|<span data-ttu-id="0bdae-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="0bdae-151">string</span></span>|<span data-ttu-id="0bdae-152">E-brödtext genereras om den här sidan visades som hello resultatet av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="0bdae-152">Email body generated if this page was displayed as hello result of an internal error.</span></span>|  
|<span data-ttu-id="0bdae-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="0bdae-153">requestedUrl</span></span>|<span data-ttu-id="0bdae-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="0bdae-154">string</span></span>|<span data-ttu-id="0bdae-155">Begärd när hello sidan inte hittades hello-URL.</span><span class="sxs-lookup"><span data-stu-id="0bdae-155">hello URL requested when hello page was not found.</span></span>|  
|<span data-ttu-id="0bdae-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="0bdae-156">referrerUrl</span></span>|<span data-ttu-id="0bdae-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="0bdae-157">string</span></span>|<span data-ttu-id="0bdae-158">hello referent URL toohello begärda URL: en.</span><span class="sxs-lookup"><span data-stu-id="0bdae-158">hello referrer URL toohello requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="0bdae-159">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0bdae-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="0bdae-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0bdae-160">Next steps</span></span>
<span data-ttu-id="0bdae-161">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0bdae-161">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
