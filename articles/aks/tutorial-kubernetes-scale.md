---
title: Samouczek dotyczący rozwiązania Kubernetes na platformie Azure — skalowanie aplikacji
description: W tym samouczku dotyczącym usługi Azure Kubernetes Service (AKS) dowiesz się, jak skalować węzły i zasobniki w rozwiązaniu Kubernetes oraz implementować automatyczne skalowanie zasobników w poziomie.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5ffe7b4c7830500e5eeeeb61c57730d9a0d9df47
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/15/2018
ms.locfileid: "41920089"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Samouczek: Skalowanie aplikacji w usłudze Azure Kubernetes Service (AKS)

Jeśli wykonujesz kolejno zadania z samouczków, masz już działający klaster Kubernetes w usłudze AKS z wdrożoną aplikacją do głosowania platformy Azure. Ta część samouczka, piąta z siedmiu, obejmuje skalowanie w poziomie zasobników w tej aplikacji oraz skalowanie automatyczne. Dowiesz się również, jak przez skalowanie liczby węzłów maszyny wirtualnej platformy Azure zmieniać możliwości hostowania obciążeń w klastrze. Omawiane kwestie:

> [!div class="checklist"]
> * Skalowanie węzłów rozwiązania Kubernetes
> * Ręczne skalowanie zasobników rozwiązania Kubernetes, w ramach których działa Twoja aplikacja
> * Konfigurowanie automatycznie skalowanych zasobników, w ramach których działa fronton aplikacji

W kolejnych samouczkach aplikacja Azure Vote jest aktualizowana do nowej wersji.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczkach aplikacja została spakowana w postaci obrazu kontenera, obraz został przekazany do usługi Azure Container Registry i utworzono klaster Kubernetes. Następnie uruchomiono aplikację w klastrze usługi Kubernetes. Jeśli te kroki nie zostały wykonane, a chcesz skorzystać z tej części samouczka, wróć do części [Samouczek 1 — tworzenie obrazów kontenera][aks-tutorial-prepare-app].

Ten samouczek wymaga interfejsu wiersza polecenia platformy Azure w wersji 2.0.38 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli-install].

## <a name="manually-scale-pods"></a>Ręczne skalowanie zasobników

W momencie wdrożenia frontonu aplikacji Azure Vote i wystąpienia pamięci podręcznej Redis w poprzednich samouczkach została utworzona pojedyncza replika. Aby wyświetlić liczbę i stan zasobników w klastrze, użyj polecenia [kubectl get][kubectl-get] w następujący sposób:

```console
kubectl get pods
```

Poniższe przykładowe dane wyjściowe zawierają jeden zasobnik frontonu i jeden zasobnik zaplecza:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Aby ręcznie zmienić liczbę zasobników w ramach wdrożenia aplikacji *azure-vote-front*, użyj polecenia [kubectl scale][kubectl-scale]. W poniższym przykładzie liczba zasobników frontonu jest zwiększana do *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Ponownie uruchom polecenie [kubectl get][kubectl-get], aby sprawdzić, czy rozwiązanie Kubernetes utworzy dodatkowe zasobniki. Po upływie około minuty dodatkowe zasobniki będą dostępne w Twoim klastrze:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Automatyczne skalowanie zasobników

Rozwiązanie Kubernetes obsługuje [automatyczne skalowanie zasobników w poziomie][kubernetes-hpa], umożliwiające dostosowywanie liczby zasobników we wdrożeniu do użycia procesora lub innych wybranych metryk. Program [Metrics Server][metrics-server] służy do zapewniania platformie Kubernetes informacji o wykorzystaniu zasobów. Aby zainstalować program Metrics Server, sklonuj repozytorium GitHub `metrics-server` i zainstaluj przykładowe definicje zasobów. Aby wyświetlić zawartość tych definicji YAML, zobacz [program Metrics Server dla platformy Kuberenetes w wersji 1.8 lub nowszej][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Aby móc korzystać ze skalowania automatycznego, należy zdefiniować wymagania i limity użycia procesora CPU dla zasobników. We wdrożeniu aplikacji `azure-vote-front` kontener frontonu wymaga 0,25 CPU, a limit wynosi 0,5 CPU. Ustawienia są następujące:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

W poniższym przykładzie użyto polecenia [kubectl autoscale][kubectl-autoscale] w celu przeprowadzenia automatycznego skalowania liczby zasobników we wdrożeniu aplikacji *azure-vote-front*. Jeśli użycie procesora CPU przekroczy 50%, skalowanie automatyczne spowoduje zwiększenie liczby zasobników do maksymalnie 10 wystąpień:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Aby wyświetlić stan skalowania automatycznego, użyj polecenia `kubectl get hpa` w następujący sposób:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Po upływie kilku minut przy minimalnym obciążeniu aplikacji Azure Vote liczba replik zasobników zostanie automatycznie zmniejszona do 3. Możesz ponownie użyć polecenia `kubectl get pods`, aby sprawdzić stan usuwania niepotrzebnych zasobników.

## <a name="manually-scale-aks-nodes"></a>Ręczne skalowanie węzłów usługi AKS

Jeśli utworzono klaster Kubernetes przy użyciu poleceń z poprzedniego samouczka, zawiera on jeden węzeł. Jeśli planujesz zwiększenie lub zmniejszenie liczby obciążeń kontenerów w klastrze, możesz ręcznie dostosować liczbę węzłów.

W poniższym przykładzie liczba węzłów w klastrze Kubernetes o nazwie *myAKSCluster* zostanie zwiększona do trzech. Wykonanie tego polecenia może zająć kilka minut.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

Dane wyjściowe są podobne do następujących:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku użyto różnych funkcji skalowania w klastrze Kubernetes. W tym samouczku omówiono:

> [!div class="checklist"]
> * Skalowanie węzłów rozwiązania Kubernetes
> * Ręczne skalowanie zasobników rozwiązania Kubernetes, w ramach których działa Twoja aplikacja
> * Konfigurowanie automatycznie skalowanych zasobników, w ramach których działa fronton aplikacji

Przejdź do następnego samouczka, aby dowiedzieć się, jak zaktualizować aplikację w rozwiązaniu Kubernetes.

> [!div class="nextstepaction"]
> [Aktualizowanie aplikacji w rozwiązaniu Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
