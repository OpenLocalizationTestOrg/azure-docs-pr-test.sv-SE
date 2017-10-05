---
title: Programmallar i Azure API Management | Microsoft Docs
description: "Lär dig hur du anpassar innehållet i programsidor i developer-portalen i Azure API Management."
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
ms.openlocfilehash: 6d2d44465800219f16866a621d4822614ac9e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-templates-in-azure-api-management"></a><span data-ttu-id="dfdd9-103">Programmallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="dfdd9-103">Application templates in Azure API Management</span></span>
<span data-ttu-id="dfdd9-104">Azure API Management ger dig möjlighet att anpassa innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="dfdd9-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="dfdd9-106">Mallarna i det här avsnittet kan du anpassa innehållet i programsidor i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-106">The templates in this section allow you to customize the content of the Application pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="dfdd9-107">Programlista</span><span class="sxs-lookup"><span data-stu-id="dfdd9-107">Application list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="dfdd9-108">Programmet</span><span class="sxs-lookup"><span data-stu-id="dfdd9-108">Application</span></span>](#Application)  
  
> [!NOTE]
>  <span data-ttu-id="dfdd9-109">Standard exempelmallarna ingår i följande dokumentation, men kan komma att ändras på grund av kontinuerliga förbättringar.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-109">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="dfdd9-110">Du kan visa live standardmallarna i developer-portalen genom att navigera till önskade enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-110">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="dfdd9-111">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="dfdd9-111">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="dfdd9-112"><a name="ProductList"></a>Programlista</span><span class="sxs-lookup"><span data-stu-id="dfdd9-112"><a name="ProductList"></a> Application list</span></span>  
 <span data-ttu-id="dfdd9-113">Den **programlista** mall kan du anpassa brödtexten i sidan program i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-113">The **Application list** template allows you to customize the body of the application list page in the developer portal.</span></span>  
  
 <span data-ttu-id="dfdd9-114">![Lista över sidan Developer Portal programmallar](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM lista sidan Developer Portal programmallar")</span><span class="sxs-lookup"><span data-stu-id="dfdd9-114">![Application List Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Application List Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="dfdd9-115">Standardmall</span><span class="sxs-lookup"><span data-stu-id="dfdd9-115">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="dfdd9-116">Kontroller</span><span class="sxs-lookup"><span data-stu-id="dfdd9-116">Controls</span></span>  
 <span data-ttu-id="dfdd9-117">Den `Product list` mall kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="dfdd9-117">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="dfdd9-118">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="dfdd9-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="dfdd9-119">Datamodell</span><span class="sxs-lookup"><span data-stu-id="dfdd9-119">Data model</span></span>  
  
|<span data-ttu-id="dfdd9-120">Egenskap</span><span class="sxs-lookup"><span data-stu-id="dfdd9-120">Property</span></span>|<span data-ttu-id="dfdd9-121">Typ</span><span class="sxs-lookup"><span data-stu-id="dfdd9-121">Type</span></span>|<span data-ttu-id="dfdd9-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dfdd9-122">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="dfdd9-123">Sidindelning</span><span class="sxs-lookup"><span data-stu-id="dfdd9-123">Paging</span></span>|<span data-ttu-id="dfdd9-124">[Växling](api-management-template-data-model-reference.md#Paging) entitet.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-124">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="dfdd9-125">Växlingsfil-information för samlingen program.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-125">The paging information for the applications collection.</span></span>|  
|<span data-ttu-id="dfdd9-126">Program</span><span class="sxs-lookup"><span data-stu-id="dfdd9-126">Applications</span></span>|<span data-ttu-id="dfdd9-127">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-127">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="dfdd9-128">Program som är synliga för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-128">The applications visible to the current user.</span></span>|  
|<span data-ttu-id="dfdd9-129">Kategorinamn</span><span class="sxs-lookup"><span data-stu-id="dfdd9-129">CategoryName</span></span>|<span data-ttu-id="dfdd9-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="dfdd9-130">string</span></span>|<span data-ttu-id="dfdd9-131">Kategori för programmet.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-131">The category of application.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="dfdd9-132">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="dfdd9-132">Sample template data</span></span>  
  
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
  
##  <span data-ttu-id="dfdd9-133"><a name="Application"></a>Programmet</span><span class="sxs-lookup"><span data-stu-id="dfdd9-133"><a name="Application"></a> Application</span></span>  
 <span data-ttu-id="dfdd9-134">Den **programmet** mall kan du anpassa brödtexten i appen på sidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-134">The **Application** template allows you to customize the body of the application page in the developer portal.</span></span>  
  
 <span data-ttu-id="dfdd9-135">![Programmet sidan Developer Portal mallar](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM sidan Developer Portal programmallar")</span><span class="sxs-lookup"><span data-stu-id="dfdd9-135">![Application Page Developer Portal Templates](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Application Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="dfdd9-136">Standardmall</span><span class="sxs-lookup"><span data-stu-id="dfdd9-136">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="dfdd9-137">Kontroller</span><span class="sxs-lookup"><span data-stu-id="dfdd9-137">Controls</span></span>  
 <span data-ttu-id="dfdd9-138">Den `Application` mallen kan inte användas för någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="dfdd9-138">The `Application` template does not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="dfdd9-139">Datamodell</span><span class="sxs-lookup"><span data-stu-id="dfdd9-139">Data model</span></span>  
 <span data-ttu-id="dfdd9-140">[Programmet](api-management-template-data-model-reference.md#Application) entitet.</span><span class="sxs-lookup"><span data-stu-id="dfdd9-140">[Application](api-management-template-data-model-reference.md#Application) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="dfdd9-141">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="dfdd9-141">Sample template data</span></span>  
  
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

## <a name="next-steps"></a><span data-ttu-id="dfdd9-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfdd9-142">Next steps</span></span>
<span data-ttu-id="dfdd9-143">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dfdd9-143">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>