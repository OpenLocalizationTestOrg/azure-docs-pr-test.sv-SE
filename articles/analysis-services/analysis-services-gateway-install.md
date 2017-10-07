---
title: aaaInstall lokala datagateway | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera en lokal datagateway."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Installera och konfigurera en gateway för lokala data
En lokal datagateway krävs när en eller flera Azure Analysis Services-servrar i hello samma region ansluta tooon lokala datakällor. toolearn mer om hello gateway finns [lokala datagateway](analysis-services-gateway.md).

## <a name="prerequisites"></a>Krav
**Minimikrav:**

* 4.5 för .NET framework
* 64-bitars version av Windows 7 / Windows Server 2008 R2 (eller senare)

**Rekommenderat:**

* 8 kärnor CPU
* 8 GB minne
* 64-bitarsversionen av Windows 2012 R2 (eller senare)

**Viktigt att tänka på:**

* Under installationen när du registrerar din gateway med Azure, har hello standardregion för din prenumeration valts. Du kan välja en annan region. Om du har servrar i flera regioner, måste du installera en gateway för varje region. 
* hello gateway kan inte installeras på en domänkontrollant.
* Endast en gateway kan installeras på en dator.
* Installera hello gateway på en dator som finns kvar på och går inte toosleep.
* Installera inte hello gateway i ett datornätverk trådlöst anslutna tooyour. Prestanda kan minskas.


## <a name="download"></a>Ladda ned
 [Hämta hello gateway](https://aka.ms/azureasgateway)

## <a name="install"></a>Installera

1. Kör installationsprogrammet.

2. Välj en plats, acceptera hello och klicka sedan på **installera**.

   ![Installera platsen och licensvillkoren](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Välj **lokala datagateway (rekommenderas)**. Azure Analysis Services stöder inte personliga läge.

   ![Välj typ av gateway](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. Ange ett konto toosign i tooAzure. hello-kontot måste vara i din klient Azure Active Directory. Det här kontot används för hello gateway-administratören. 

   ![Ange ett konto toosign i tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Om du loggar in med ett domänkonto, kommer att mappas tooyour organisationens konto i Azure AD. Ditt organisationskonto används som hello hello gateway-administratören.

## <a name="register"></a>Registrera dig
Du måste registrera hello lokal instans som du installerat med hello Gateway Cloud Service i ordning toocreate en gateway-resurs i Azure. 

1.  Välj **registrera en ny gateway på den här datorn**.

    ![Registrera dig](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Skriv ett namn och återställa nyckeln för din gateway. Som standard använder hello gateway standardregion för din prenumeration. Om du behöver tooselect en annan region, Välj **ändra Region**.

   ![Registrera dig](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-resource"></a>Skapa en Azure gateway-resurs
När du har installerat och registrerat din gateway, måste toocreate en gateway-resurs i din Azure-prenumeration. Logga in tooAzure med hello samma konto som du använde när du registrerar hello gateway.

1. I Azure-portalen klickar du på **skapa en ny tjänst** > **Enterprise Integration** > **lokala datagateway**  >   **Skapa**.

   ![Skapa en gateway-resurs](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. I **skapa anslutningen gateway**, ange dessa inställningar:

    * **Namnet**: Ange ett namn för din gateway-resurs. 

    * **Prenumerationen**: Välj hello Azure-prenumeration tooassociate med gateway-resurs. 
    Den här prenumerationen ska vara hello samma prenumeration som servrarna finns i.
   
      Hej standardabonnemang baseras på hello Azure-konto som du använde toosign i.

    * **Resursgrupp**: Skapa en resursgrupp eller välj en befintlig resursgrupp.

    * **Plats**: Välj hello region som du har registrerat din gateway i.

    * **Installationen namnet**: om gatewayinstallationen av inte är markerat, Välj hello gatewayen har registrerats. 

    När du är klar klickar du på **skapa**.

## <a name="connect-servers"></a>Anslut servrar toohello gatewayresursen

1. I din Azure Analysis Services översikt över server, klickar du på **lokala Data Gateway**.

   ![Anslut servern toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. I **Välj en lokala Data Gateway tooconnect**, väljer du gateway-resurs och klicka sedan på **Anslut markerad gateway**.

   ![Anslut servern toogateway resursen](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Om din gateway inte visas i listan hello din server är förmodligen inte hello samma region som hello region som du angav när du registrerar hello gateway. 

Det var allt. Om du behöver tooopen portar eller göra felsökning vara säker på att toocheck ut [lokala datagateway](analysis-services-gateway.md).

## <a name="next-steps"></a>Nästa steg
* [Hantera Analysis Services](analysis-services-manage.md)   
* [Hämta data från Azure Analysis Services](analysis-services-connect.md)
