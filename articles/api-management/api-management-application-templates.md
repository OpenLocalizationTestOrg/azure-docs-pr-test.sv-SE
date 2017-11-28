---
title: aaaApplication mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur toocustomize hello innehållet i hello programsidor i hello developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: f4dc078be7163b047ca2e640a9065ba9e5ba709e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-templates-in-azure-api-management"></a><span data-ttu-id="77011-103">Programmallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="77011-103">Application templates in Azure API Management</span></span>
<span data-ttu-id="77011-104">Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="77011-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="77011-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="77011-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="77011-106">hello mallar i det här avsnittet kan du toocustomize hello innehållet i hello programsidor i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="77011-106">hello templates in this section allow you toocustomize hello content of hello Application pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="77011-107">Programlista</span><span class="sxs-lookup"><span data-stu-id="77011-107">Application list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="77011-108">Programmet</span><span class="sxs-lookup"><span data-stu-id="77011-108">Application</span></span>](#Application)  
  
> [!NOTE]
>  <span data-ttu-id="77011-109">Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar.</span><span class="sxs-lookup"><span data-stu-id="77011-109">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="77011-110">Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="77011-110">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="77011-111">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="77011-111">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="77011-112"><a name="ProductList"></a>Programlista</span><span class="sxs-lookup"><span data-stu-id="77011-112"><a name="ProductList"></a> Application list</span></span>  
 <span data-ttu-id="77011-113">Hej **programlista** mall kan du toocustomize hello brödtext hello programmet sidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="77011-113">hello **Application list** template allows you toocustomize hello body of hello application list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="77011-114">![Lista över sidan Developer Portal programmallar](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM lista sidan Developer Portal programmallar")</span><span class="sxs-lookup"><span data-stu-id="77011-114">![Application List Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Application List Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="77011-115">Standardmall</span><span class="sxs-lookup"><span data-stu-id="77011-115">Default template</span></span>  
  
```xml  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "AppStrings|WebApplicationsHeader" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if applications.size > 0 %}  
        <ul class="list-unstyled">  
        {% for app in applications %}  
            <li>  
            {% if app.application.icon.url != "" %}  
                <aside>  
                    <a href="/applications/details/{{app.application.id}}"><img src="{{app.application.icon.url}}" alt="App Icon" /></a>  
                </aside>  
            {% endif %}  
                <h3><a href="/applications/details/{{app.application.id}}">{{app.application.title}}</a></h3>  
                {{app.application.description}}  
            </li>     
        {% endfor %}  
        </ul>  
        <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="77011-116">Kontroller</span><span class="sxs-lookup"><span data-stu-id="77011-116">Controls</span></span>  
 <span data-ttu-id="77011-117">Hej `Product list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="77011-117">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="77011-118">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="77011-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="77011-119">Datamodell</span><span class="sxs-lookup"><span data-stu-id="77011-119">Data model</span></span>  
  
|<span data-ttu-id="77011-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="77011-120">Property</span></span>|<span data-ttu-id="77011-121">Typ</span><span class="sxs-lookup"><span data-stu-id="77011-121">Type</span></span>|<span data-ttu-id="77011-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="77011-122">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="77011-123">Sidindelning</span><span class="sxs-lookup"><span data-stu-id="77011-123">Paging</span></span>|<span data-ttu-id="77011-124">[Växling](api-management-template-data-model-reference.md#Paging) entitet.</span><span class="sxs-lookup"><span data-stu-id="77011-124">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="77011-125">hello sidindelning information för hello program samling.</span><span class="sxs-lookup"><span data-stu-id="77011-125">hello paging information for hello applications collection.</span></span>|  
|<span data-ttu-id="77011-126">Program</span><span class="sxs-lookup"><span data-stu-id="77011-126">Applications</span></span>|<span data-ttu-id="77011-127">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="77011-127">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="77011-128">hello program visas toohello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="77011-128">hello applications visible toohello current user.</span></span>|  
|<span data-ttu-id="77011-129">Kategorinamn</span><span class="sxs-lookup"><span data-stu-id="77011-129">CategoryName</span></span>|<span data-ttu-id="77011-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="77011-130">string</span></span>|<span data-ttu-id="77011-131">hello kategori av programmet.</span><span class="sxs-lookup"><span data-stu-id="77011-131">hello category of application.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="77011-132">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="77011-132">Sample template data</span></span>  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Applications": [  
        {  
            "Application": {  
                "Id": "5702b96fb16653124c8f9ba8",  
                "Title": "Contoso Calculator",  
                "Description": "A simple online calculator.",  
                "Url": null,  
                "Version": null,  
                "Requirements": "Free application with no requirements.",  
                "State": 2,  
                "RegistrationDate": "2016-04-04T18:59:00",  
                "CategoryId": 5,  
                "DeveloperId": "5702b5b0b16653124c8f9ba4",  
                "Attachments": [  
                    {  
                        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                        "Type": "Icon",  
                        "ContentType": "image/png"  
                    },  
                    {  
                        "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
                        "Type": "Screenshot",  
                        "ContentType": "image/png"  
                    }  
                ],  
                "Icon": {  
                    "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                    "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                    "Type": "Icon",  
                    "ContentType": "image/png"  
                }  
            },  
            "CategoryName": "Finance"  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="77011-133"><a name="Application"></a>Programmet</span><span class="sxs-lookup"><span data-stu-id="77011-133"><a name="Application"></a> Application</span></span>  
 <span data-ttu-id="77011-134">Hej **programmet** mall kan du toocustomize hello brödtext hello appen på sidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="77011-134">hello **Application** template allows you toocustomize hello body of hello application page in hello developer portal.</span></span>  
  
 <span data-ttu-id="77011-135">![Programmet sidan Developer Portal mallar](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM sidan Developer Portal programmallar")</span><span class="sxs-lookup"><span data-stu-id="77011-135">![Application Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Application Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="77011-136">Standardmall</span><span class="sxs-lookup"><span data-stu-id="77011-136">Default template</span></span>  
  
```xml  
<h2>{{title}}</h2>  
{% if icon.url != "" %}  
<aside class="applications_aside">  
  <div class="image-placeholder">  
    <img src="{{icon.url}}" alt="Application Icon" />  
  </div>  
</aside>  
{% endif %}  
  
<article>  
  {% if url != "" %}  
    <a target="_blank" href="{{url}}">{{url}}</a>  
  {% endif %}  
  
  <p>{{description}}</p>  
  
  {% if requirements != null %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsRequirementsHeader" %}</h3>  
  <p>{{requirements}}</p>  
  {% endif %}  
  
  {% if attachments.size > 0 %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsScreenshotsHeader" %}</h3>  
    {% for screenshot in attachments %}  
      {% if screenshot.type != "Icon" %}  
      <a href="{{screenshot.url}}" data-lightbox="example-set">  
          <img src="/Developer/Applications/Thumbnail?url={{screenshot.url}}" alt='{% localized "AppDetailsStrings|WebApplicationsScreenshotAlt" %}' />  
      </a>  
      {% endif %}  
    {% endfor %}  
  {% endif %}  
</article>  
  
```  
  
### <a name="controls"></a><span data-ttu-id="77011-137">Kontroller</span><span class="sxs-lookup"><span data-stu-id="77011-137">Controls</span></span>  
 <span data-ttu-id="77011-138">Hej `Application` mall tillåter inte hello användning av något [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="77011-138">hello `Application` template does not allow hello use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="77011-139">Datamodell</span><span class="sxs-lookup"><span data-stu-id="77011-139">Data model</span></span>  
 <span data-ttu-id="77011-140">[Programmet](api-management-template-data-model-reference.md#Application) entitet.</span><span class="sxs-lookup"><span data-stu-id="77011-140">[Application](api-management-template-data-model-reference.md#Application) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="77011-141">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="77011-141">Sample template data</span></span>  
  
```json  
{  
    "Id": "5702b96fb16653124c8f9ba8",  
    "Title": "Contoso Calculator",  
    "Description": "A simple online calculator.",  
    "Url": null,  
    "Version": null,  
    "Requirements": "Free application with no requirements.",  
    "State": 2,  
    "RegistrationDate": "2016-04-04T18:59:00",  
    "CategoryId": 5,  
    "DeveloperId": "5702b5b0b16653124c8f9ba4",  
    "Attachments": [  
        {  
            "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
            "Type": "Icon",  
            "ContentType": "image/png"  
        },  
        {  
            "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
            "Type": "Screenshot",  
            "ContentType": "image/png"  
        }  
    ],  
    "Icon": {  
        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
        "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
        "Type": "Icon",  
        "ContentType": "image/png"  
    }  
}  
```

## <a name="next-steps"></a><span data-ttu-id="77011-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77011-142">Next steps</span></span>
<span data-ttu-id="77011-143">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="77011-143">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
