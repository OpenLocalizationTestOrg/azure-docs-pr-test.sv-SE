---
title: aaaAzure hanterat program skapa UI definition funktioner | Microsoft Docs
description: "Beskriver hello funktioner toouse vid UI definitioner för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: a34c6202372168cda769c471b1c9fdd539dd0f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition element
Den här artikeln beskriver hello schemat och egenskaperna för alla element som stöds av en CreateUiDefinition. Du använder de här elementen när [att skapa ett Azure hanterade program](managed-application-publishing.md). hello-schemat för de flesta element är följande:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit hello [Azure Portal](portal.azure.com).",
  "constraints": {},
  "options": {},
  "visible": true
}
```
| Egenskap | Krävs | Beskrivning |
| -------- | -------- | ----------- |
| namn | Ja | En intern identifierare tooreference en specifik instans av ett element. hello vanligaste användning av hello elementnamn är i `outputs`, där hello utdatavärden för hello angetts element är mappade toohello parametrar för hello mall. Du kan också använda den toobind hello utdatavärdet för ett element toohello `defaultValue` av ett annat element. |
| typ | Ja | hello UI styra toorender för hello-elementet. En lista över typer som stöds, se [element](#elements). |
| Etikett | Ja | hello visa texten i hello-element. Vissa elementtyper av innehåller flera etiketter hello värdet kan vara ett objekt som innehåller flera strängar. |
| Standardvärde | Nej | hello standardvärdet hello-element. Vissa elementtyper har stöd för komplexa standardvärden så hello värdet kan vara ett objekt. |
| Knappbeskrivning | Nej | hello text toodisplay i hello verktygstips för hello-element. Liknande för`label`, vissa element stöd för flera verktyget tips strängar. Infogade länkar inbäddade med markdownsyntax.
| Villkor | Nej | En eller flera egenskaper som används toocustomize hello valideringsbeteendet hello-element. hello stöds egenskaper för begränsningar varierar beroende på elementtyp. Vissa elementtyper av gör inte stöd för anpassning av hello valideringsbeteende och därför har ingen egenskap med begränsningar. |
| alternativ | Nej | Ytterligare egenskaper som anpassar hello funktionssätt hello-element. Liknande för`constraints`, hello stöds egenskaper varierar beroende på elementtyp. |
| Synliga | Nej | Anger om hello element ska visas. Om `true`, visas hello element och tillhörande underordnade element. hello standardvärdet är `true`. Använd [logiska funktioner](managed-application-createuidefinition-functions.md#logical-functions) toodynamically styr den här egenskapens värde.

## <a name="elements"></a>Element

Hej dokumentationen för varje element innehåller ett exempel på UI, schemat, anmärkning på hello beteendet för hello element (vanligtvis om verifiering och anpassning som stöds) och exempel på utdata.

- [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
- [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Nästa steg
* En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
