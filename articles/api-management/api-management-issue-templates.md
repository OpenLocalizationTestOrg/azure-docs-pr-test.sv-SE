---
title: "Utfärda mallar i Azure API Management | Microsoft Docs"
description: "Lär dig hur du anpassar innehållet problemet sidor i developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e13344df198bca4f73c75fa58221436b94e2f258
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="12076-103">Utfärda mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="12076-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="12076-104">Azure API Management ger dig möjlighet att anpassa innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="12076-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="12076-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="12076-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="12076-106">Mallarna i det här avsnittet kan du anpassa innehållet problemet sidor i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="12076-106">The templates in this section allow you to customize the content of the Issue pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="12076-107">Problemet lista</span><span class="sxs-lookup"><span data-stu-id="12076-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="12076-108">Standard exempelmallarna ingår i följande dokumentation, men kan komma att ändras på grund av kontinuerliga förbättringar.</span><span class="sxs-lookup"><span data-stu-id="12076-108">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="12076-109">Du kan visa live standardmallarna i developer-portalen genom att navigera till önskade enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="12076-109">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="12076-110">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="12076-110">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="12076-111"><a name="IssueList"></a>Problemet lista</span><span class="sxs-lookup"><span data-stu-id="12076-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="12076-112">Den **problemet listan** mall kan du anpassa sidan problemet till utvecklarportalen brödtext.</span><span class="sxs-lookup"><span data-stu-id="12076-112">The **Issue list** template allows you to customize the body of the issue list page in the developer portal.</span></span>  
  
 <span data-ttu-id="12076-113">![Utfärda listan Developer-portalen](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM problemet listan Developer-portalen")</span><span class="sxs-lookup"><span data-stu-id="12076-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="12076-114">Standardmall</span><span class="sxs-lookup"><span data-stu-id="12076-114">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-md-9">  
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>  
  </div>  
</div>  
<div class="row">  
  <div class="col-md-12">  
    {% if issues.size > 0 %}  
    <ul class="list-unstyled">  
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}  
      {% assign replaceString0 = '{0}' %}  
      {% assign replaceString1 = '{1}' %}  
      {% for issue in issues %}  
      <li>  
        <h3>  
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>  
        </h3>  
        <p>{{issue.description}}</p>  
        <em>  
          {% capture state %}{{issue.issueState}}{% endcapture %}  
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}  
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}  
          {{ str1 | replace : replaceString1, devName }}  
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>  
        </em>  
      </li>  
      {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    {% if canReportIssue %}  
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>  
    {% elsif isAuthenticated %}  
    <hr />  
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>  
    {% else %}  
    <hr />  
    <p>  
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}  
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}  
      {{ signIntext | replace : replaceString0, link }}  
    </p>  
    {% endif %}  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="12076-115">Kontroller</span><span class="sxs-lookup"><span data-stu-id="12076-115">Controls</span></span>  
 <span data-ttu-id="12076-116">Den `Issue list` mall kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="12076-116">The `Issue list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="12076-117">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="12076-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="12076-118">Datamodell</span><span class="sxs-lookup"><span data-stu-id="12076-118">Data model</span></span>  
  
|<span data-ttu-id="12076-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="12076-119">Property</span></span>|<span data-ttu-id="12076-120">Typ</span><span class="sxs-lookup"><span data-stu-id="12076-120">Type</span></span>|<span data-ttu-id="12076-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="12076-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="12076-122">Problem</span><span class="sxs-lookup"><span data-stu-id="12076-122">Issues</span></span>|<span data-ttu-id="12076-123">Samling av [problemet](api-management-template-data-model-reference.md#Issue) entiteter.</span><span class="sxs-lookup"><span data-stu-id="12076-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="12076-124">Problem som är synliga för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="12076-124">The issues visible to the current user.</span></span>|  
|<span data-ttu-id="12076-125">Sidindelning</span><span class="sxs-lookup"><span data-stu-id="12076-125">Paging</span></span>|<span data-ttu-id="12076-126">[Växling](api-management-template-data-model-reference.md#Paging) entitet.</span><span class="sxs-lookup"><span data-stu-id="12076-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="12076-127">Växlingsfil-information för samlingen program.</span><span class="sxs-lookup"><span data-stu-id="12076-127">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="12076-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="12076-128">IsAuthenticated</span></span>|<span data-ttu-id="12076-129">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="12076-129">boolean</span></span>|<span data-ttu-id="12076-130">Om den aktuella användaren är inloggad på developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="12076-130">Whether the current user is signed-in to the developer portal.</span></span>|  
|<span data-ttu-id="12076-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="12076-131">CanReportIssues</span></span>|<span data-ttu-id="12076-132">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="12076-132">boolean</span></span>|<span data-ttu-id="12076-133">Om den aktuella användaren har behörigheter till filen ett problem.</span><span class="sxs-lookup"><span data-stu-id="12076-133">Whether the current user has permissions to file an issue.</span></span>|  
|<span data-ttu-id="12076-134">Söka</span><span class="sxs-lookup"><span data-stu-id="12076-134">Search</span></span>|<span data-ttu-id="12076-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="12076-135">string</span></span>|<span data-ttu-id="12076-136">Den här egenskapen är inaktuell och bör inte användas.</span><span class="sxs-lookup"><span data-stu-id="12076-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="12076-137">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="12076-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how to connect my application to the API",  
            "Description": "I'm having trouble connecting my application to the backend API.",  
            "SubscriptionDeveloperName": "Clayton",  
            "IssueState": "Proposed",  
            "ReportedOn": "2016-04-04T18:46:35.64",  
            "Comments": null,  
            "Attachments": null,  
            "Services": null  
        }  
    ],  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "IsAuthenticated": true,  
    "CanReportIssue": true,  
    "Search": null  
}  
```

## <a name="next-steps"></a><span data-ttu-id="12076-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12076-138">Next steps</span></span>
<span data-ttu-id="12076-139">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="12076-139">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>