---
title: "aaa ”användarens profilmallar i Azure API Management | Microsoft Docs ”"
description: "Lär dig hur toocustomize hello innehållet i hello användarprofil sidor i hello developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2e3b73ef-d223-44fe-9280-c3af3fd4a030
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: c8f153b310221164809acf58e4af236928ceb41d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="0e243-103">Användarens profilmallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0e243-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="0e243-104">Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="0e243-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="0e243-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="0e243-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="0e243-106">hello mallar i det här avsnittet kan du toocustomize hello innehåll hello användarens profil sidor i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e243-106">hello templates in this section allow you toocustomize hello content of hello User profile pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="0e243-107">Profil</span><span class="sxs-lookup"><span data-stu-id="0e243-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="0e243-108">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0e243-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="0e243-109">Program</span><span class="sxs-lookup"><span data-stu-id="0e243-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="0e243-110">Uppdatera kontoinformation</span><span class="sxs-lookup"><span data-stu-id="0e243-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="0e243-111">Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar.</span><span class="sxs-lookup"><span data-stu-id="0e243-111">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="0e243-112">Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="0e243-112">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="0e243-113">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="0e243-113">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="0e243-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="0e243-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="0e243-115">Hej **profil** mall kan du toocustomize hello avsnitt för användarprofil av hello användaren profilsida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e243-115">hello **profile** template allows you toocustomize hello user profile section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0e243-116">![Användaren profilsida](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM användarprofilen sida")</span><span class="sxs-lookup"><span data-stu-id="0e243-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0e243-117">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0e243-117">Default template</span></span>  
  
```xml  
<div class="pull-right">  
  {% if canChangePassword == true %}  
  <a class="btn btn-default" id="ChangePassword" role="button" href="{{changePasswordUrl}}">{% localized "UserProfile|ButtonLabelChangePassword" %}</a>  
  {% endif %}  
  <a id="changeAccountInfo" href="{{changeNameOrEmailUrl}}" class="btn btn-default">  
    <span class="glyphicon glyphicon-user"></span>  
    <span>{% localized "UserProfile|ButtonLabelChangeAccountInfo" %}</span>  
  </a>  
</div>  
<h2>{% localized "SubscriptionStrings|PageTitleDeveloperProfile" %}</h2>  
<div class="container-fluid">  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="Email">{% localized "UserProfile|TextboxLabelEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="Email">{{email}}</div>  
  </div>  
  
  {% if isSystemUser != true %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="FirstName">{% localized "UserProfile|TextboxLabelEmailFirstName" %}</label>  
    </div>  
    <div class="col-sm-9" id="FirstName">{{FirstName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="LastName">{% localized "UserProfile|TextboxLabelEmailLastName" %}</label>  
    </div>  
    <div class="col-sm-9" id="LastName">{{LastName}}</div>  
  </div>  
  {% else %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="CompanyName">{% localized "UserProfile|TextboxLabelOrganizationName" %}</label>  
    </div>  
    <div class="col-sm-9" id="CompanyName">{{CompanyName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="AddresserEmail">{% localized "UserProfile|TextboxLabelNotificationsSenderEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="AddresserEmail">{{AddresserEmail}}</div>  
  </div>  
  {% endif %}  
  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="0e243-118">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0e243-118">Controls</span></span>  
 <span data-ttu-id="0e243-119">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0e243-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="0e243-120">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0e243-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0e243-121">Hej [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar hello samma data modell och ta emot hello samma malldata.</span><span class="sxs-lookup"><span data-stu-id="0e243-121">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="0e243-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e243-122">Property</span></span>|<span data-ttu-id="0e243-123">Typ</span><span class="sxs-lookup"><span data-stu-id="0e243-123">Type</span></span>|<span data-ttu-id="0e243-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e243-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="0e243-125">Förnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-125">firstName</span></span>|<span data-ttu-id="0e243-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-126">string</span></span>|<span data-ttu-id="0e243-127">Hello aktuella användarens förnamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-127">First name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-128">Efternamn</span><span class="sxs-lookup"><span data-stu-id="0e243-128">lastName</span></span>|<span data-ttu-id="0e243-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-129">string</span></span>|<span data-ttu-id="0e243-130">Hello aktuella användarens efternamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-130">Last name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-131">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-131">companyName</span></span>|<span data-ttu-id="0e243-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-132">string</span></span>|<span data-ttu-id="0e243-133">hello företagsnamn för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-133">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="0e243-134">addresserEmail</span></span>|<span data-ttu-id="0e243-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-135">string</span></span>|<span data-ttu-id="0e243-136">Hello aktuella användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0e243-136">Email address of hello current user.</span></span>|  
|<span data-ttu-id="0e243-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="0e243-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="0e243-138">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-138">string</span></span>|<span data-ttu-id="0e243-139">Relativ URL tooview analytics för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-139">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="0e243-140">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0e243-140">subscriptions</span></span>|<span data-ttu-id="0e243-141">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="0e243-142">hello prenumerationer för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-142">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="0e243-143">program</span><span class="sxs-lookup"><span data-stu-id="0e243-143">applications</span></span>|<span data-ttu-id="0e243-144">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="0e243-145">hello program för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-145">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="0e243-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-146">changePasswordUrl</span></span>|<span data-ttu-id="0e243-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-147">string</span></span>|<span data-ttu-id="0e243-148">hello relativ URL toochange hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-148">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="0e243-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="0e243-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-150">string</span></span>|<span data-ttu-id="0e243-151">Hej relativ URL toochange hello-namn och e-post för den aktuella användaren i hello.</span><span class="sxs-lookup"><span data-stu-id="0e243-151">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="0e243-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="0e243-152">canChangePassword</span></span>|<span data-ttu-id="0e243-153">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-153">boolean</span></span>|<span data-ttu-id="0e243-154">Om hello aktuella användare kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-154">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="0e243-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="0e243-155">isSystemUser</span></span>|<span data-ttu-id="0e243-156">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-156">boolean</span></span>|<span data-ttu-id="0e243-157">Om hello aktuella användaren är medlem i en hello inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="0e243-157">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="0e243-158">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0e243-158">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="0e243-159"><a name="Subscriptions"></a>Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0e243-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="0e243-160">Hej **prenumerationer** mall kan du toocustomize hello prenumerationer avsnitt i hello användaren profilsida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e243-160">hello **Subscriptions** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0e243-161">![Användaren prenumerationssidan](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM användarsidan prenumeration")</span><span class="sxs-lookup"><span data-stu-id="0e243-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0e243-162">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0e243-162">Default template</span></span>  
  
```xml  
<div class="ap-account-subscriptions">  
  <a href="{{developersUsageStatisticsLink}}" id="UsageStatistics" class="btn btn-default pull-right">  
    <span class="glyphicon glyphicon-stats"></span>  
    <span>{% localized "SubscriptionListStrings|WebDevelopersUsageStatisticsLink" %}</span>  
  </a>  
  
  <h2>{% localized "SubscriptionListStrings|WebDevelopersYourSubscriptions" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th>Subscription details</th>  
        <th>Product</th>  
        <th>{% localized "SubscriptionListStrings|WebDevelopersSubscriptionTableStateHeader" %}</th>  
        <th>Action</th>  
      </tr>  
    </thead>  
    <tbody>  
      {% if subscriptions.size == 0 %}  
      <tr>  
        <td class="text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
      {% else %}  
      {% for subscription in subscriptions %}  
      <tr id="{{subscription.id}}" {% if subscription.hasExpired %} class="expired" {% endif %}>  
        <td>  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelName" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.displayName }}  
            </div>  
            <div class="col-lg-2">  
              <a class="btn-link" href="/Subscriptions/{{subscription.id}}/Rename">Rename</a>  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          {% if subscription.isAwaitingApproval %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelRequestedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.createdDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% else %}  
          {% if subscription.isRejected == false %}  
          {% if subscription.startDate %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelStartedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.startDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% endif %}  
  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.primaryKey}}', '{{subscription.id}}', true) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersPrimaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="primary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <!-- ko if: !requestInProgress() -->  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="togglePrimary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel"></a>  
                |  
                <a href="#" class="btn-link" id="regeneratePrimary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel"></a>  
              </div>  
              <!-- /ko -->  
              <!-- ko if: requestInProgress() -->  
              <div class="progress progress-striped active">  
                <div class="progress-bar" role="progressbar" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100" style="width: 100%">  
                  <span class="sr-only"></span>  
                </div>  
              </div>  
              <!-- /ko -->  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          <!-- /ko -->  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.secondaryKey}}', '{{subscription.id}}', false) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersSecondaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="secondary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="toggleSecondary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel">{% localized "SubscriptionListStrings|ButtonLabelShowKey" %}</a>  
                |  
                <a href="#" class="btn-link" id="regenerateSecondary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel">{% localized "SubscriptionListStrings|WebDevelopersRegenerateLink" %}</a>  
              </div>  
            </div>  
            <div class="clearfix"> </div>  
          </div>  
          <!-- /ko -->  
          {% endif %}  
          {% endif %}  
        </td>  
        <td>  
          <a href="{{subscription.productDetailsUrl}}">{{subscription.productTitle}}</a>  
        </td>  
        <td>  
          <strong>  
            {{subscription.state}}  
          </strong>  
        </td>  
        <td>  
          <div class="nowrap">  
            {% if subscription.canBeCancelled %}  
            <subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }"></subscription-cancel>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="0e243-163">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0e243-163">Controls</span></span>  
 <span data-ttu-id="0e243-164">Den här mallen kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0e243-164">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="0e243-165">Avbryt prenumeration</span><span class="sxs-lookup"><span data-stu-id="0e243-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="0e243-166">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0e243-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0e243-167">Hej [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar hello samma data modell och ta emot hello samma malldata.</span><span class="sxs-lookup"><span data-stu-id="0e243-167">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="0e243-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e243-168">Property</span></span>|<span data-ttu-id="0e243-169">Typ</span><span class="sxs-lookup"><span data-stu-id="0e243-169">Type</span></span>|<span data-ttu-id="0e243-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e243-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="0e243-171">Förnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-171">firstName</span></span>|<span data-ttu-id="0e243-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-172">string</span></span>|<span data-ttu-id="0e243-173">Hello aktuella användarens förnamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-173">First name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="0e243-174">lastName</span></span>|<span data-ttu-id="0e243-175">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-175">string</span></span>|<span data-ttu-id="0e243-176">Hello aktuella användarens efternamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-176">Last name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-177">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-177">companyName</span></span>|<span data-ttu-id="0e243-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-178">string</span></span>|<span data-ttu-id="0e243-179">hello företagsnamn för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-179">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="0e243-180">addresserEmail</span></span>|<span data-ttu-id="0e243-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-181">string</span></span>|<span data-ttu-id="0e243-182">Hello aktuella användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0e243-182">Email address of hello current user.</span></span>|  
|<span data-ttu-id="0e243-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="0e243-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="0e243-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-184">string</span></span>|<span data-ttu-id="0e243-185">Relativ URL tooview analytics för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-185">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="0e243-186">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0e243-186">subscriptions</span></span>|<span data-ttu-id="0e243-187">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="0e243-188">hello prenumerationer för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-188">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="0e243-189">program</span><span class="sxs-lookup"><span data-stu-id="0e243-189">applications</span></span>|<span data-ttu-id="0e243-190">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="0e243-191">hello program för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-191">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="0e243-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-192">changePasswordUrl</span></span>|<span data-ttu-id="0e243-193">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-193">string</span></span>|<span data-ttu-id="0e243-194">hello relativ URL toochange hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-194">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="0e243-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="0e243-196">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-196">string</span></span>|<span data-ttu-id="0e243-197">Hej relativ URL toochange hello-namn och e-post för den aktuella användaren i hello.</span><span class="sxs-lookup"><span data-stu-id="0e243-197">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="0e243-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="0e243-198">canChangePassword</span></span>|<span data-ttu-id="0e243-199">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-199">boolean</span></span>|<span data-ttu-id="0e243-200">Om hello aktuella användare kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-200">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="0e243-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="0e243-201">isSystemUser</span></span>|<span data-ttu-id="0e243-202">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-202">boolean</span></span>|<span data-ttu-id="0e243-203">Om hello aktuella användaren är medlem i en hello inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="0e243-203">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="0e243-204">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0e243-204">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="0e243-205"><a name="Applications"></a>Program</span><span class="sxs-lookup"><span data-stu-id="0e243-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="0e243-206">Hej **program** mall kan du toocustomize hello prenumerationer avsnitt i hello användaren profilsida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e243-206">hello **Applications** template allows you toocustomize hello subscriptions section of hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0e243-207">![Användarsida konto program](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM användarkontot Programsidan")</span><span class="sxs-lookup"><span data-stu-id="0e243-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0e243-208">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0e243-208">Default template</span></span>  
  
```xml  
<div class="ap-account-applications">  
  <a id="RegisterApplication" href="/Developer/Applications/Register" class="btn btn-success pull-right">  
    <span class="glyphicon glyphicon-plus"></span>  
    <span>{% localized "ApplicationListStrings|WebDevelopersRegisterAppLink" %}</span>  
  </a>  
  <h2>{% localized "ApplicationListStrings|WebDevelopersYourApplicationsHeader" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th class="col-md-8">{% localized "ApplicationListStrings|WebDevelopersAppTableNameHeader" %}</th>  
        <th class="col-md-2">{% localized "ApplicationListStrings|WebDevelopersAppTableCategoryHeader" %}</th>  
        <th class="col-md-2" colspan="2">{% localized "ApplicationListStrings|WebDevelopersAppTableStateHeader" %}</th>  
      </tr>  
    </thead>  
    <tbody>  
  
      {% if applications.size == 0 %}  
  
      <tr>  
        <td class="col-md-12 text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
  
      {% else %}  
  
      {% for app in applications %}  
      <tr>  
        <td class="col-md-8">  
          {{app.title}}  
        </td>  
        <td class="col-md-2">  
          {{app.categoryName}}  
        </td>  
        <td class="col-md-2">  
          <strong>  
            {% case app.state %}  
            {% when ApplicationStateModel.Registered %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotSubminted" %}  
  
            {% when ApplicationStateModel.Unpublished %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotPublished" %}  
  
            {% else %}  
            {{ app.state }}  
            {% endcase %}  
          </strong>  
        </td>  
        <td class="col-md-1">  
          <div class="nowrap">  
            {% if app.state != ApplicationStateModel.Submitted and app.state != ApplicationStateModel.Published %}  
            <app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="0e243-209">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0e243-209">Controls</span></span>  
 <span data-ttu-id="0e243-210">Den här mallen kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0e243-210">This template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="0e243-211">App-åtgärder</span><span class="sxs-lookup"><span data-stu-id="0e243-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="0e243-212">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0e243-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0e243-213">Hej [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar hello samma data modell och ta emot hello samma malldata.</span><span class="sxs-lookup"><span data-stu-id="0e243-213">hello [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share hello same data model and receive hello same template data.</span></span>  
  
|<span data-ttu-id="0e243-214">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e243-214">Property</span></span>|<span data-ttu-id="0e243-215">Typ</span><span class="sxs-lookup"><span data-stu-id="0e243-215">Type</span></span>|<span data-ttu-id="0e243-216">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e243-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="0e243-217">Förnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-217">firstName</span></span>|<span data-ttu-id="0e243-218">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-218">string</span></span>|<span data-ttu-id="0e243-219">Hello aktuella användarens förnamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-219">First name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-220">Efternamn</span><span class="sxs-lookup"><span data-stu-id="0e243-220">lastName</span></span>|<span data-ttu-id="0e243-221">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-221">string</span></span>|<span data-ttu-id="0e243-222">Hello aktuella användarens efternamn.</span><span class="sxs-lookup"><span data-stu-id="0e243-222">Last name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-223">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="0e243-223">companyName</span></span>|<span data-ttu-id="0e243-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-224">string</span></span>|<span data-ttu-id="0e243-225">hello företagsnamn för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-225">hello company name of hello current user.</span></span>|  
|<span data-ttu-id="0e243-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="0e243-226">addresserEmail</span></span>|<span data-ttu-id="0e243-227">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-227">string</span></span>|<span data-ttu-id="0e243-228">Hello aktuella användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0e243-228">Email address of hello current user.</span></span>|  
|<span data-ttu-id="0e243-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="0e243-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="0e243-230">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-230">string</span></span>|<span data-ttu-id="0e243-231">Relativ URL tooview analytics för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-231">Relative URL tooview analytics for hello current user.</span></span>|  
|<span data-ttu-id="0e243-232">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0e243-232">subscriptions</span></span>|<span data-ttu-id="0e243-233">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="0e243-234">hello prenumerationer för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-234">hello subscriptions for hello current user.</span></span>|  
|<span data-ttu-id="0e243-235">program</span><span class="sxs-lookup"><span data-stu-id="0e243-235">applications</span></span>|<span data-ttu-id="0e243-236">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="0e243-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="0e243-237">hello program för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="0e243-237">hello applications of hello current user.</span></span>|  
|<span data-ttu-id="0e243-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-238">changePasswordUrl</span></span>|<span data-ttu-id="0e243-239">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-239">string</span></span>|<span data-ttu-id="0e243-240">hello relativ URL toochange hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-240">hello relative URL toochange hello current user's password.</span></span>|  
|<span data-ttu-id="0e243-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="0e243-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="0e243-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="0e243-242">string</span></span>|<span data-ttu-id="0e243-243">Hej relativ URL toochange hello-namn och e-post för den aktuella användaren i hello.</span><span class="sxs-lookup"><span data-stu-id="0e243-243">hello relative URL toochange hello name and email for hello current user.</span></span>|  
|<span data-ttu-id="0e243-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="0e243-244">canChangePassword</span></span>|<span data-ttu-id="0e243-245">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-245">boolean</span></span>|<span data-ttu-id="0e243-246">Om hello aktuella användare kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="0e243-246">Whether hello current user can change their password.</span></span>|  
|<span data-ttu-id="0e243-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="0e243-247">isSystemUser</span></span>|<span data-ttu-id="0e243-248">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="0e243-248">boolean</span></span>|<span data-ttu-id="0e243-249">Om hello aktuella användaren är medlem i en hello inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="0e243-249">Whether hello current user is a member of one of hello built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="0e243-250">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0e243-250">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="0e243-251"><a name="UpdateAccountInfo"></a>Uppdatera kontoinformation</span><span class="sxs-lookup"><span data-stu-id="0e243-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="0e243-252">Hej **Uodate kontoinformation** mall kan du toocustomize hello **uppdatera kontoinformation** sida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="0e243-252">hello **Uodate account info** template allows you toocustomize hello **Update account information** page in hello developer portal.</span></span>  
  
 <span data-ttu-id="0e243-253">![Information om sidan Developer Portal mallar för användarkonton](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM Info sidan Developer Portal mallar för användarkonton")</span><span class="sxs-lookup"><span data-stu-id="0e243-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="0e243-254">Standardmall</span><span class="sxs-lookup"><span data-stu-id="0e243-254">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-sm-6 col-md-6">  
    <div class="form-group">  
      <label for="Email">{% localized "SigninResources|TextboxLabelEmail" %}</label>  
      <input autofocus="autofocus" class="form-control" id="Email" name="Email" type="text" value="{{email}}">  
    </div>  
    <div class="form-group">  
      <label for="FirstName">{% localized "SigninResources|TextboxLabelEmailFirstName" %}</label>  
      <input class="form-control" id="FirstName" name="FirstName" type="text" value="{{firstName}}">  
    </div>  
    <div class="form-group">  
      <label for="LastName">{% localized "SigninResources|TextboxLabelEmailLastName" %}</label>  
      <input class="form-control" id="LastName" name="LastName" type="text" value="{{lastName}}">  
    </div>  
    <div class="form-group">  
      <label for="Password">{% localized "SigninResources|WebAuthenticationSigninPasswordLabel" %}</label>  
      <input class="form-control" id="Password" name="Password" type="password">  
    </div>  
  </div>  
</div>  
  
<button type="submit" class="btn btn-primary" id="UpdateProfile">  
  {% localized "UpdateProfileStrings|ButtonLabelUpdateProfile" %}  
</button>  
<a class="btn btn-default" href="/developer" role="button">  
  {% localized "CommonStrings|ButtonLabelCancel" %}  
</a>  
```  
  
### <a name="controls"></a><span data-ttu-id="0e243-255">Kontroller</span><span class="sxs-lookup"><span data-stu-id="0e243-255">Controls</span></span>  
 <span data-ttu-id="0e243-256">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="0e243-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="0e243-257">Datamodell</span><span class="sxs-lookup"><span data-stu-id="0e243-257">Data model</span></span>  
 <span data-ttu-id="0e243-258">[Information om användarkonto](api-management-template-data-model-reference.md#UserAccountInfo) entitet.</span><span class="sxs-lookup"><span data-stu-id="0e243-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="0e243-259">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="0e243-259">Sample template data</span></span>  
  
```json  
{  
    "FirstName": "Administrator",  
    "LastName": "",  
    "Email": "admin@live.com",  
    "Password": null,  
    "NameIdentifier": null,  
    "ProviderName": null,  
    "IsBasicAccount": false  
}  
```

## <a name="next-steps"></a><span data-ttu-id="0e243-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e243-260">Next steps</span></span>
<span data-ttu-id="0e243-261">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0e243-261">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
