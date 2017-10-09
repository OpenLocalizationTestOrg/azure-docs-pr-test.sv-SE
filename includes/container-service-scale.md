# <a name="scale-agent-nodes-in-a-container-service-cluster"></a>Skala agentnoder i ett behållartjänstkluster
Efter [distribuera ett Azure Container Service-kluster](../articles/container-service/dcos-swarm/container-service-deployment.md), måste du kanske toochange hello antalet agent noder. Till exempel kan du behöva fler agenter så att du kan köra flera behållarprogram eller instanser. 

Du kan ändra hello antalet agent noder i ett kluster för DC/OS, Docker Swarm eller Kubernetes med hello Azure-portalen eller hello Azure CLI 2.0. 

## <a name="scale-with-hello-azure-portal"></a>Skala med hello Azure-portalen

1. I hello [Azure-portalen](https://portal.azure.com), bläddra efter **behållartjänster**, och klicka sedan på hello behållartjänsten som du vill toomodify.
2. I hello **behållartjänsten** bladet, klickar du på **agenter**.
3. I **antal VM**, ange hello önskat antal agenter noder.

    ![Skala en pool i hello-portalen](./media/container-service-scale/container-service-scale-portal.png)

4. toosave hello konfiguration, klickar du på **spara**.

## <a name="scale-with-hello-azure-cli-20"></a>Skala med hello Azure CLI 2.0

Se till att du [installerat](/cli/azure/install-az-cli2) hello senaste Azure CLI 2.0 och loggas i tooan azure-konto (`az login`).

### <a name="see-hello-current-agent-count"></a>Se hello aktuella agentantal
toosee hello antalet agenter som för närvarande i hello klustret kör hello `az acs show` kommando. Detta visar hello klusterkonfigurationen. Hej exempelvis följande kommando visar hello configuration hello container Service med namnet `containerservice-myACSName` i hello resursgruppen `myResourceGroup`:

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

hello kommandot returnerar hello antalet agenter i hello `Count` värde `AgentPoolProfiles`.

### <a name="use-hello-az-acs-scale-command"></a>Använd hello az acs skala kommando
toochange hello antal agent noder, kör hello `az acs scale` kommandot och varor hello **resursgruppen**, **service behållarnamn**, och hello önskad **nya agentantal**. Om du använder ett lägre eller högre antal kan du skala ned eller upp.

Till exempel toochange hello antalet agenter i hello tidigare kluster too10, typen hello följande kommando:

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

hello Azure CLI 2.0 returnerar en JSON-sträng som representerar hello ny konfiguration hello container Service, inklusive antal för nya hello-agenten.

Om du vill ha fler kommandoalternativ kör du `az acs scale --help`.

## <a name="scaling-considerations"></a>Att tänka på vid skalning

* hello antalet agent noder måste vara mellan 1 och 100. 

* Din kärnor kvot kan begränsa hello antalet agent noder i ett kluster.

* Åtgärder för agenten nod skalning är tillämpade tooan Azure virtuella datorns skaluppsättning som innehåller hello agent poolen. I ett DC/OS-kluster skalas endast agent noder i hello privata pool av hello åtgärder visas i den här artikeln.

* Beroende på hello orchestrator som du distribuerar i ditt kluster, kan du separat skala hello antal instanser av en behållare som körs på klustret hello. Till exempel använda hello i ett DC/OS-kluster [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello antal instanser av ett program för behållare.

* För närvarande går det inte att autoskala agentnoder i ett behållartjänstkluster.

## <a name="next-steps"></a>Nästa steg
* Se [fler exempel](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) på att använda kommandon i Azure CLI 2.0 med Azure Container Service.
* Läs mer om [DC/OS-agentpooler](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) i Azure Container Service.

