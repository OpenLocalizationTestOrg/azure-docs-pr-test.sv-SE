Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter parametrar som innehåller alla hello parametervärden.
Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som behålls alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras. 

När du definierar parametrar, använda hello **allowedValues** fältet toospecify som värden en användare kan ange under distributionen. Använd hello **defaultValue** fältet tooassign värdet toohello parameter, om inget värde anges under distributionen.

Vi beskriver varje parameter i hello mallen.

### <a name="sitename"></a>Platsnamn
hello namnet på hello webbprogram som du vill toocreate.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
hello namnet på hello Apptjänst planera toouse som värd för hello webbprogrammet.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
hello prisnivån för hello som värd för planen.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "hello pricing tier for hello hosting plan."
      }
    }

hello mallen definierar hello värden som tillåts för den här parametern och tilldelas ett standardvärde (S1) om inget värde anges.

### <a name="workersize"></a>workerSize
Hej instansstorleken för hello värd plan (liten, medel eller hög).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

hello mallen definierar hello värden som tillåts för den här parametern (0, 1 eller 2) och tilldelas ett standardvärde (0) om inget värde anges. hello värden motsvarar toosmall, medelstora och stora.

