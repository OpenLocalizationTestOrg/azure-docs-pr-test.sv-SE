## <a name="set-up-azure-cli-for-azure-dns"></a>Konfigurera Azure CLI för Azure DNS

### <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har hello följande objekt innan du börjar din konfiguration.

* En Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Installera hello senaste versionen av hello Azure CLI, tillgänglig för Windows, Linux och MAC. Mer information finns på [installera hello Azure CLI](../articles/cli-install-nodejs.md).

### <a name="sign-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto

Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter. Mer information finns i [logga in tooAzure från hello Azure CLI](../articles/xplat-cli-connect.md)

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Växla CLI-läge

Azure DNS använder Azure Resource Manager. Kontrollera att du växla CLI läge toouse Azure Resource Manager-kommandon.

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a>Välj hello-prenumeration

Kontrollera hello prenumerationer för hello-kontot.

```azurecli
azure account list
```

Välj vilka av dina Azure-prenumerationer toouse.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Resource Manager kräver att alla resursgrupper anger en plats. Detta används som hello standardplatsen för resurser i resursgruppen. Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.

Du kan hoppa över det här steget om du använder en befintlig resursgrupp.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Registrera resursprovider

hello Azure DNS-tjänst som hanteras av hello Microsoft.Network-resursprovidern. Din Azure-prenumeration måste vara registrerade toouse den här resursprovidern innan du kan använda Azure DNS. Det här är en engångsåtgärd för varje prenumeration.

```azurecli
azure provider register --namespace Microsoft.Network
```

