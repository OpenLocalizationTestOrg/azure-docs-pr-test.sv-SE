---
title: "aaaAzure hanterade program FileUpload gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Common.FileUpload UI-element för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 7af5bec992e3f120afb1bdf56d8b4c19a8e5e834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonfileupload-ui-element"></a><span data-ttu-id="3a37b-103">Microsoft.Common.FileUpload UI-element</span><span class="sxs-lookup"><span data-stu-id="3a37b-103">Microsoft.Common.FileUpload UI element</span></span>
<span data-ttu-id="3a37b-104">En kontroll som gör att en användare toospecify en eller flera filer tooupload.</span><span class="sxs-lookup"><span data-stu-id="3a37b-104">A control that allows a user toospecify one or more files tooupload.</span></span> <span data-ttu-id="3a37b-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3a37b-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="3a37b-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="3a37b-106">UI sample</span></span>
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a><span data-ttu-id="3a37b-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="3a37b-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Common.FileUpload",
  "label": "Some file upload",
  "toolTip": "",
  "constraints": {
    "required": true,
    "accept": ".doc,.docx,.xml,application/msword"
  },
  "options": {
    "multiple": false,
    "uploadMode": "file",
    "openMode": "text",
    "encoding": "UTF-8"
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="3a37b-109">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="3a37b-109">Remarks</span></span>
- <span data-ttu-id="3a37b-110">`constraints.accept`Anger hello filtyper som visas i hello webbläsarens fildialogruta.</span><span class="sxs-lookup"><span data-stu-id="3a37b-110">`constraints.accept` specifies hello types of files that are shown in hello browser's file dialog.</span></span> <span data-ttu-id="3a37b-111">Se hello [HTML5-specifikationen](http://www.w3.org/TR/html5/forms.html#attr-input-accept) för tillåtna värden.</span><span class="sxs-lookup"><span data-stu-id="3a37b-111">See hello [HTML5 specification](http://www.w3.org/TR/html5/forms.html#attr-input-accept) for allowed values.</span></span> <span data-ttu-id="3a37b-112">hello standardvärdet är **null**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-112">hello default value is **null**.</span></span>
- <span data-ttu-id="3a37b-113">Om `options.multiple` har angetts för**SANT**, hello användaren tillåts tooselect mer än en fil i hello webbläsarens fildialogruta.</span><span class="sxs-lookup"><span data-stu-id="3a37b-113">If `options.multiple` is set too**true**, hello user is allowed tooselect more than one file in hello browser's file dialog.</span></span> <span data-ttu-id="3a37b-114">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-114">hello default value is **false**.</span></span>
- <span data-ttu-id="3a37b-115">Det här elementet stöder Överför filer i två lägen, baserat på hello värdet för `options.uploadMode`.</span><span class="sxs-lookup"><span data-stu-id="3a37b-115">This element supports uploading files in two modes based on hello value of `options.uploadMode`.</span></span> <span data-ttu-id="3a37b-116">Om **filen** anges hello utdata innehåller hello innehållet i hello-filen som en blob.</span><span class="sxs-lookup"><span data-stu-id="3a37b-116">If **file** is specified, hello output contains hello contents of hello file as a blob.</span></span> <span data-ttu-id="3a37b-117">Om **url** anges hello filen överförda tooa tillfällig plats, och hello utdata innehåller hello URL för hello-blob.</span><span class="sxs-lookup"><span data-stu-id="3a37b-117">If **url** is specified, then hello file is uploaded tooa temporary location, and hello output contains hello URL of hello blob.</span></span> <span data-ttu-id="3a37b-118">Tillfällig blobbar ska rensas efter 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="3a37b-118">Temporary blobs will be purged after 24 hours.</span></span> <span data-ttu-id="3a37b-119">hello standardvärdet är **filen**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-119">hello default value is **file**.</span></span>
- <span data-ttu-id="3a37b-120">Hej värdet för `options.openMode` avgör hur hello filen läses.</span><span class="sxs-lookup"><span data-stu-id="3a37b-120">hello value of `options.openMode` determines how hello file is read.</span></span> <span data-ttu-id="3a37b-121">Ange om hello-filen är förväntade toobe oformaterad text **text**; annan, ange **binär**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-121">If hello file is expected toobe plain text, specify **text**; else, specify **binary**.</span></span> <span data-ttu-id="3a37b-122">hello standardvärdet är **text**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-122">hello default value is **text**.</span></span>
- <span data-ttu-id="3a37b-123">Om `options.uploadMode` har angetts för**filen** och `options.openMode` har angetts för**binära**, hello utdata är base64-kodad.</span><span class="sxs-lookup"><span data-stu-id="3a37b-123">If `options.uploadMode` is set too**file** and `options.openMode` is set too**binary**, hello output is base64-encoded.</span></span>
- <span data-ttu-id="3a37b-124">`options.encoding`Anger hello kodning toouse vid läsning av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3a37b-124">`options.encoding` specifies hello encoding toouse when reading hello file.</span></span> <span data-ttu-id="3a37b-125">hello standardvärdet är **UTF-8**, och används bara när `options.openMode` har angetts för**text**.</span><span class="sxs-lookup"><span data-stu-id="3a37b-125">hello default value is **UTF-8**, and is used only when `options.openMode` is set too**text**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="3a37b-126">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="3a37b-126">Sample output</span></span>
<span data-ttu-id="3a37b-127">Om options.multiple är false och options.uploadMode är filen innehåller utdata hello innehållet i hello-filen som en JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="3a37b-127">If options.multiple is false and options.uploadMode is file, then the output contains hello contents of hello file as a JSON string:</span></span>

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

<span data-ttu-id="3a37b-128">Om options.multiple är true and'options.uploadMode är fil och sedan utdata innehåller hello innehållet i hello-filer som en JSON-matris:</span><span class="sxs-lookup"><span data-stu-id="3a37b-128">If options.multiple is true and\`options.uploadMode is file, then the output contains hello contents of hello files as a JSON array:</span></span>

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

<span data-ttu-id="3a37b-129">Om options.multiple är false och options.uploadMode är url innehåller resultatet en URL som en JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="3a37b-129">If options.multiple is false and options.uploadMode is url, then the output contains a URL as a JSON string:</span></span>

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

<span data-ttu-id="3a37b-130">Om options.multiple är true och options.uploadMode är url innehåller resultatet en lista över webbadresser som en JSON-matris:</span><span class="sxs-lookup"><span data-stu-id="3a37b-130">If options.multiple is true and options.uploadMode is url, then the output contains a list of URLs as a JSON array:</span></span>
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

<span data-ttu-id="3a37b-131">När du testar en CreateUiDefinition trunkera vissa webbläsare (till exempel Google Chrome) URL: er som genererats av hello Microsoft.Common.FileUpload element i hello webbläsarens konsol.</span><span class="sxs-lookup"><span data-stu-id="3a37b-131">When testing a CreateUiDefinition, some browsers (like Google Chrome) truncate URLs generated by hello Microsoft.Common.FileUpload element in hello browser console.</span></span> <span data-ttu-id="3a37b-132">Du måste kanske klicka tooright på enskilda länkar toocopy hello fullständig URL-adresser.</span><span class="sxs-lookup"><span data-stu-id="3a37b-132">You may need tooright-click individual links toocopy hello full URLs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3a37b-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a37b-133">Next steps</span></span>
* <span data-ttu-id="3a37b-134">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a37b-134">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3a37b-135">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a37b-135">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="3a37b-136">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="3a37b-136">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
