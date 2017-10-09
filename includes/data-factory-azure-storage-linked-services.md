### <a name="azure-storage-linked-service"></a>Länkad Azure Storage-tjänst
Hej **Azure länkade lagringstjänsten** kan du toolink ett Azure storage-konto tooan Azure data factory med hjälp av hello **kontonyckel**, vilket ger hello data factory med global åtkomst toohello Azure Lagring. hello följande tabell ger en beskrivning för JSON-element specifika tooAzure länkade lagringstjänsten.

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |hello Typegenskapen måste anges till: **AzureStorage** |Ja |
| connectionString |Ange nödvändig information tooconnect tooAzure lagring för hello-egenskapen connectionString. |Ja |

Se följande artikel för steg tooview/kopiera hello konto nyckeln för ett Azure Storage hello: [visa, kopiera och generera lagring åtkomstnycklar](../articles/storage/common/storage-create-storage-account.md#manage-your-storage-account).

**Exempel:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

### <a name="azure-storage-sas-linked-service"></a>Azure Storage Sas länkade tjänsten
En signatur för delad åtkomst (SAS) ger delegerad åtkomst tooresources i ditt lagringskonto. Det gör att toogrant en klient begränsade behörigheter tooobjects i ditt lagringskonto för en angiven tidsperiod och med en angiven uppsättning behörigheter, utan att behöva tooshare åtkomstnycklarna för ditt konto. hello SAS är en URI som omfattar alla hello uppgifter som är nödvändiga för autentiserad åtkomst tooa lagringsresurs i dess Frågeparametrar. tooaccess lagringsresurser med hello SAS hello klienten behöver bara toopass i hello SAS toohello lämplig konstruktor eller metod. Detaljerad information om SAS finns [signaturer för delad åtkomst: Förstå hello SAS-modellen](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory nu har bara stöd för **Service SAS** men inte kontots SAS. Se [typer av signaturer för delad åtkomst](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) mer information om dessa två typer och hur tooconstruct. Observera hello SAS-URL generable från Azure-portalen eller Lagringsutforskaren är ett konto-SAS, vilket inte stöds.
> 

hello Azure Storage SAS länkad tjänst kan du toolink ett Azure Storage-konto tooan Azure data factory med hjälp av en delad signatur åtkomst (SAS). Det ger hello data factory med begränsad/Tidsbundna åtkomst tooall utvalda resurser (blobbehållare) i hello lagring. hello följande tabell ger en beskrivning för JSON-element specifika tooAzure lagring SAS länkade tjänsten. 

| Egenskap | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ |hello Typegenskapen måste anges till: **AzureStorageSas** |Ja |
| sasUri |Ange URI för delad åtkomst-signatur toohello Azure Storage-resurser, till exempel blob, behållare eller tabellen.  |Ja |

**Exempel:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<Specify SAS URI of hello Azure Storage resource>"   
        }  
    }  
}  
```

När du skapar en **SAS URI**, överväger hello följande:  

* Ange lämpliga läsning och skrivning **behörigheter** på objekt baserat på hur hello länkade tjänsten (läsa, skriva, läsning och skrivning) används i din data factory.
* Ange **förfallotiden** på lämpligt sätt. Kontrollera att hello åtkomst tooAzure lagringsobjekt inte upphör hello aktiva perioden för hello pipeline.
* URI: N ska skapas på hello rätt behållare/blob eller tabell nivå utifrån hello måste. SAS-Uri-tooan Azure blob kan hello Data Factory-tjänsten tooaccess viss blobben. En SAS-Uri tooan Azure blob-behållare kan hello Data Factory-tjänsten tooiterate via blobbar i behållaren. Om du behöver fler tooprovide åtkomst/färre objekt senare eller uppdatera hello SAS URI, Kom ihåg tooupdate hello länkad tjänst med hello ny URI.   

