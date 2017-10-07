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
# <a name="microsoftcommonfileupload-ui-element"></a>Microsoft.Common.FileUpload UI-element
En kontroll som gör att en användare toospecify en eller flera filer tooupload. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Common.FileUpload](./media/managed-application-elements/microsoft.common.fileupload.png)

## <a name="schema"></a>Schemat
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

## <a name="remarks"></a>Kommentarer
- `constraints.accept`Anger hello filtyper som visas i hello webbläsarens fildialogruta. Se hello [HTML5-specifikationen](http://www.w3.org/TR/html5/forms.html#attr-input-accept) för tillåtna värden. hello standardvärdet är **null**.
- Om `options.multiple` har angetts för**SANT**, hello användaren tillåts tooselect mer än en fil i hello webbläsarens fildialogruta. hello standardvärdet är **FALSKT**.
- Det här elementet stöder Överför filer i två lägen, baserat på hello värdet för `options.uploadMode`. Om **filen** anges hello utdata innehåller hello innehållet i hello-filen som en blob. Om **url** anges hello filen överförda tooa tillfällig plats, och hello utdata innehåller hello URL för hello-blob. Tillfällig blobbar ska rensas efter 24 timmar. hello standardvärdet är **filen**.
- Hej värdet för `options.openMode` avgör hur hello filen läses. Ange om hello-filen är förväntade toobe oformaterad text **text**; annan, ange **binär**. hello standardvärdet är **text**.
- Om `options.uploadMode` har angetts för**filen** och `options.openMode` har angetts för**binära**, hello utdata är base64-kodad.
- `options.encoding`Anger hello kodning toouse vid läsning av hello-filen. hello standardvärdet är **UTF-8**, och används bara när `options.openMode` har angetts för**text**.

## <a name="sample-output"></a>Exempel på utdata
Om options.multiple är false och options.uploadMode är filen innehåller utdata hello innehållet i hello-filen som en JSON-sträng:

```json
"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
```

Om options.multiple är true and'options.uploadMode är fil och sedan utdata innehåller hello innehållet i hello-filer som en JSON-matris:

```json
[
  "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
  "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
  "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
  "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
]
```

Om options.multiple är false och options.uploadMode är url innehåller resultatet en URL som en JSON-sträng:

```json
"https://myaccount.blob.core.windows.net/pictures/profile.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
```

Om options.multiple är true och options.uploadMode är url innehåller resultatet en lista över webbadresser som en JSON-matris:
```json
[
  "https://myaccount.blob.core.windows.net/pictures/profile1.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile2.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d",
  "https://myaccount.blob.core.windows.net/pictures/profile3.jpg?sv=2013-08-15&st=2013-08-16&se=2013-08-17&sr=c&sp=r&rscd=file;%20attachment&rsct=binary &sig=YWJjZGVmZw%3d%3d&sig=a39%2BYozJhGp6miujGymjRpN8tsrQfLo9Z3i8IRyIpnQ%3d"
]
```

När du testar en CreateUiDefinition trunkera vissa webbläsare (till exempel Google Chrome) URL: er som genererats av hello Microsoft.Common.FileUpload element i hello webbläsarens konsol. Du måste kanske klicka tooright på enskilda länkar toocopy hello fullständig URL-adresser.


## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
