Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras. hello mallen innehåller ett avsnitt som heter parametrar som innehåller alla hello parametervärden.
Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till. Definiera inte parametrar för värden som behålls alltid hello samma. Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras. 

När du definierar parametrar, använda hello **allowedValues** fältet toospecify som värden en användare kan ange under distributionen. Använd hello **defaultValue** fältet tooassign värdet toohello parameter, om inget värde anges under distributionen.

Vi beskriver varje parameter i hello mallen.

### <a name="logicappname"></a>logicAppName
hello namnet på hello logik app toocreate.

    "logicAppName": {
        "type": "string"
    }