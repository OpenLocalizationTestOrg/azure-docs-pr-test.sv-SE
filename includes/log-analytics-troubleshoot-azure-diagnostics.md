### <a name="troubleshoot-azure-diagnostics"></a>Felsöka Azure Diagnostics

Om du får följande felmeddelande hello är hello Microsoft.insights resursprovidern inte registrerad:

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

tooregister hello resursleverantör, utföra hello följa stegen i hello Azure-portalen:

1.  Hello navigeringsfönstret hello vänster, klicka på *prenumerationer*
2.  Välj hello-prenumeration som identifieras i hello felmeddelande
3.  Klicka på *Resursprovidrar*
4.  Hitta hello *Microsoft.insights* provider
5.  Klicka på hello *registrera* länk

![Registrera resursprovidern microsoft.insights](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

En gång hello *Microsoft.insights* resursprovidern har registrerats och försök sedan att konfigurera diagnostik.


I PowerShell om du får följande felmeddelande hello, måste tooupdate din version av PowerShell:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Uppdatera din version av PowerShell toohello November 2016 (v2.3.0) eller senare, frigöra använder hello instruktioner i hello [Kom igång med Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) artikel.
