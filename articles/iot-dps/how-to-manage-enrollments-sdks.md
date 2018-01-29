---
title: Hantera enhetsregistrering med Azure enheten etablering Service SDK | Microsoft Docs
description: "Så här hanterar du enhetsregistrering i IoT-hubb enheten Etableringstjänsten med SDK: er för tjänsten"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 12/01/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: arjmands
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 14e353af82342bc7a580e1a0a02b8b4e29514fb9
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/17/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Så här hanterar du enhetsregistrering med Azure enheten etablering Service-SDK: er
En *enhetsregistrering* skapas en post för en enstaka enhet eller en grupp av enheter som kan vid något tillfälle registrera med enheten Etableringstjänsten. Registreringspost innehåller inledande önskad konfiguration för enheter som en del av att registreringen, inklusive önskade IoT-hubben. Den här artikeln visar hur du hanterar enhetsregistreringar för etablering tjänsten genom programmering med Azure IoT etablering Service SDK: erna.  SDK: erna är tillgängliga på GitHub i samma databas som Azure IoT-SDK.

## <a name="samples"></a>Exempel
Den här artikeln går igenom hög nivå begrepp för att hantera enhetsregistreringar för etablering tjänsten genom programmering med Azure IoT etablering Service SDK: erna.  Exakt API-anrop kan skilja på grund av språkskillnader.  Läs igenom exemplen som vi tillhandahåller på GitHub för ytterligare information:
* [Exempel på Java-etablering-klienten](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
* [Node.js etablering Tjänstklienten prover](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
* [-Klienten för etablering i .NET-exempel](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="prerequisites"></a>Förutsättningar
* Anslutningssträng från en enhet etablering tjänstinstans
* Enheten säkerhet artefakter:
    * [**TPM**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Registrering av enskilda: registrerings-ID och TPM-bekräftelsenyckel från en fysisk enhet eller TPM-simulatorn.
        * Registrering grupp gäller inte för attestering av TPM.
    * [**X.509**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Registrering av enskilda: den [lövcertifikatet](https://docs.microsoft.com/azure/iot-dps/concepts-security#leaf-certificate) från fysisk enhet eller RAPPORTANVÄNDARNA emulatorn.
        * Grupp för registrering: den [rotcertifikat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) eller [mellanliggande certifikat](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate), som används för att skapa certifikat på en fysisk enhet.  Det kan också genereras från RAPPORTANVÄNDARNA emulatorn.

## <a name="create-a-device-enrollment"></a>Skapa en registrering av enheter

Det finns två sätt som du kan registrera dina enheter med tjänsten etablering:

* En **registrering grupp** finns en post för en grupp av enheter som delar en gemensam attestering mekanismen för X.509-certifikat som signerats av den [rotcertifikat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) eller [mellanliggande certifikat ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Vi rekommenderar en grupp för registrering för ett stort antal enheter som delar en önskad inledande konfiguration eller för enheter alla kommer att samma klientorganisation. Observera att du bara kan registrera enheter som använder mekanismen för attestering X.509 som *registrering grupper*. 

    Du kan skapa en grupp för registrering med SDK: erna efter det här arbetsflödet:

    1. Mekanismen för attestering använder X.509-rotcertifikat för registrering av grupp.  Anropa API för SDK-tjänsten ```X509Attestation.createFromRootCertificate``` med rotcertifikat för att skapa intyg för registrering.  X.509-rotcertifikat finns i antingen en PEM-fil eller som en sträng.
    1. Skapa en ny ```EnrollmentGroup``` variabeln med den ```attestation``` skapade och en unik ```enrollmentGroupId```.  Du kan ange parametrar som ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Anropa API för SDK-tjänsten ```createOrUpdateEnrollmentGroup``` i backend-programmet med ```EnrollmentGroup``` att skapa en grupp för registrering.

* En **enskilda registrering** finns en post för en enda enhet som kan registrera. Enskilda registreringar kan använda X.509-certifikat eller SAS-token (i TPM verkliga eller virtuella) som mekanismer för attestering. Vi rekommenderar att du använder enskilda registreringar för enheter som kräver unika första konfigurationer eller för enheter som bara kan använda SAS-token via TPM eller virtuella TPM som mekanismen för attestering. Enskilda registreringar kanske önskade IoT-hubb enhets-ID angetts.

    Du kan skapa en enskild registrering med SDK: erna efter det här arbetsflödet:
    
    1. Välj din ```attestation``` mekanism som kan vara TPM eller X.509.
        1. **TPM**: använda bekräftelsenyckeln från en fysisk enhet eller TPM Simulator som indata, du kan anropa API för SDK-tjänsten ```TpmAttestation``` att skapa intyg för registrering. 
        2. **X.509**: använder klientcertifikatet som indata, kan du anropa tjänsten SDK API ```X509Attestation.createFromClientCertificate``` att skapa intyg för registrering.
    2. Skapa en ny ```IndividualEnrollment``` variabeln med den ```attestation``` skapade och en unik ```registrationId``` som indata som på enheten eller genereras från TPM-simulatorn.  Du kan ange parametrar som ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Anropa API för SDK-tjänsten ```createOrUpdateIndividualEnrollment``` i backend-programmet med ```IndividualEnrollment``` att skapa en enskild registrering.

När du har skapat en registrering av returnerar enhet Etableringstjänsten ett resultat för registrering.

Det här arbetsflödet visar den [exempel](#samples).

## <a name="update-an-enrollment-entry"></a>Uppdatera en post för registrering

När du har skapat en post för registrering, kanske du vill uppdatera registreringen.  Möjliga scenarier är uppdatering av önskad egenskap, uppdatera metoden attestering eller återkalla Enhetsåtkomst.  Det finns olika API: er för enskilda registrering och gruppen registrering, men ingen åtskillnad för attestering mekanism.

Du kan uppdatera en post för registrering efter det här arbetsflödet:
* **Enskild registrering**:
    1. Hämta den senaste registreringen från tjänsten etablering första med tjänsten SDK API ```getIndividualEnrollment```.
    2. Ändra parametern till den senaste registreringen vid behov. 
    3. Med den senaste registreringen anropa tjänsten SDK API ```createOrUpdateIndividualEnrollment``` att uppdatera inmatningen registrering.
* **Gruppen registrering**:
    1. Hämta den senaste registreringen från tjänsten etablering första med tjänsten SDK API ```getEnrollmentGroup```.
    2. Ändra parametern till den senaste registreringen vid behov.
    3. Med den senaste registreringen anropa tjänsten SDK API ```createOrUpdateEnrollmentGroup``` att uppdatera inmatningen registrering.

Det här arbetsflödet visar den [exempel](#samples).

## <a name="remove-an-enrollment-entry"></a>Ta bort en post för registrering

* **Registrering av enskilda** kan tas bort genom att anropa API för SDK-tjänsten ```deleteIndividualEnrollment``` med ```registrationId```.
* **Gruppen registrering** kan tas bort genom att anropa API för SDK-tjänsten ```deleteEnrollmentGroup``` med ```enrollmentGroupId```.

Det här arbetsflödet visar den [exempel](#samples).

## <a name="bulk-operation-on-individual-enrollments"></a>Massåtgärd på enskilda registreringar

Du kan utföra massåtgärd för att skapa, uppdatera eller ta bort flera enskilda registreringar efter det här arbetsflödet:

1. Skapa en variabel som innehåller flera ```IndividualEnrollment```.  Implementering av den här variabeln är olika för varje språk.  Granska bulk åtgärden exemplet på GitHub för ytterligare information.
2. Anropa API för SDK-tjänsten ```runBulkOperation``` med en ```BulkOperationMode``` för den önskade åtgärden och variabeln för enskilda registreringar. Fyra lägen stöds: skapa, uppdatera, updateIfMatchEtag, och ta bort.

När du har har utfört en åtgärd, skulle enheten Etableringstjänsten returnera resultat en grupp.

Det här arbetsflödet visar den [exempel](#samples).
