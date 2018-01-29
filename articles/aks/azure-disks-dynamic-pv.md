---
title: "Använd Azure med AKS"
description: "Använda Azure-diskarna med AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Beständiga volymer med Azure-diskarna - dynamiska etablering

En beständig volym representerar en typ av lagring som har etablerats för användning i ett Kubernetes kluster. En beständig volym kan användas av en eller flera skida och kan etableras statiskt eller dynamiskt. Det här dokumentet beskriver dynamisk etablering av en Azure-disken som Kubernetes beständiga volymer i ett AKS kluster. 

Mer information om Kubernetes beständiga volymer finns [Kubernetes beständiga volymer][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Inbyggda lagringsklasser

En lagringsklass används för att definiera hur en dynamiskt skapade beständiga volym är konfigurerat. Mer information om Kubernetes lagringsklasser finns [Kubernetes lagringsklasser][kubernetes-storage-classes].

Varje AKS kluster innehåller två lagringsklasser som skapats i förväg, båda är konfigurerade för att fungera med Azure-diskarna. Använd den `kubectl get storageclass` kommandot för att se dessa.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Om dessa lagringsklasser fungerar för dina behov kan behöver du inte skapa en ny.

## <a name="create-storage-class"></a>Skapa storage-klass

Om du vill skapa en ny lagringsklass som konfigurerats för Azure-diskar kan du göra det med följande exempel manifestet. 

Den `storageaccounttype` värdet för `Standard_LRS` anger att en disk som standard skapas. Det här värdet kan ändras till `Premium_LRS` att skapa en [premium disk][premium-storage]. Om du vill använda premiumdiskar måste AKS nod virtuell dator ha en storlek som är kompatibelt med premiumdiskar. Se [dokumentet] [ premium-storage] för en lista över kompatibla storlekar.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Skapa beständiga volym anspråk

En beständig volym anspråk använder ett lagringsobjekt för klassen för att dynamiskt etablerar en typ av lagring. När du använder en Azure-disken, skapas disken i samma resursgrupp som AKS resurser.

Manifestet för det här exemplet skapar en beständig volym anspråk med hjälp av den `azure-managed-disk` lagringsklass, skapa en disk `5GB` i storlek med `ReadWriteOnce` åtkomst. Mer information om PVC Åtkomstlägen finns [Åtkomstlägen][access-modes].

> [!NOTE]
> En Azure-disken kan endast monteras med åtkomst läge typen ReadWriteOnce, vilket gör den tillgänglig att endast en AKS nod. Om du behöver att dela en beständig volym över flera noder, kan du använda [Azure Files][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Med hjälp av beständiga volymens

När anspråket beständiga volymen har skapats och den disk som etablerats, en baljor kan skapas med åtkomst till disken. Följande manifestet skapar en baljor som använder beständiga volym anspråket `azure-managed-disk` Azure disken på den `/var/www/html` sökväg. 

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Nästa steg

Läs mer om Kubernetes beständiga volymer med Azure-diskarna.

> [!div class="nextstepaction"]
> [Kubernetes plugin-program för Azure-diskar][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md