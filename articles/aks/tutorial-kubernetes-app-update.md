---
title: Samouczek dotyczący usługi Kubernetes na platformie Azure — aktualizowanie aplikacji
description: Z tego samouczka dotyczącego usługi Azure Kubernetes Service (AKS) dowiesz się, jak zaktualizować istniejące wdrożenie aplikacji do usługi AKS przy użyciu nowej wersji kodu aplikacji.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b2dd52fec112b879e072d3ac5598dd7978e68cbc
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919557"
---
# <a name="tutorial-update-an-application-in-azure-kubernetes-service-aks"></a>Samouczek: aktualizowanie aplikacji w usłudze Azure Kubernetes Service (AKS)

Po wdrożeniu aplikacji w usłudze Kubernetes można ją zmodyfikować, określając nowy obraz kontenera lub wersję obrazu. W takiej sytuacji aktualizacja jest przygotowywana tak, aby tylko część była współbieżnie aktualizowana. Ta aktualizacja etapowa umożliwia kontynuowanie działania podczas aktualizacji aplikacji. Udostępnia ona również mechanizm wycofywania w przypadku niepowodzenia wdrożenia.

W tym samouczku (część szósta z siedmiu) aktualizowana jest przykładowa aplikacja do głosowania platformy Azure. Omawiane kwestie:

> [!div class="checklist"]
> * Aktualizowanie kodu aplikacji frontonu
> * Tworzenie zaktualizowanego obrazu kontenera
> * Wypychanie obrazu kontenera do usługi Azure Container Registry
> * Wdrażanie zaktualizowanego obrazu kontenera

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczkach aplikacja została spakowana w postaci obrazu kontenera, obraz został przekazany do usługi Azure Container Registry i utworzono klaster Kubernetes. Następnie uruchomiono aplikację w klastrze usługi Kubernetes.

Sklonowano również repozytorium aplikacji, w tym kod źródłowy aplikacji i utworzony wcześniej plik narzędzia Docker Compose używany w tym samouczku. Sprawdź, czy został utworzony klon repozytorium oraz czy katalogi zostały zmienione na sklonowany katalog. Jeśli nie wykonano tych kroków, a chcesz kontynuować pracę, wróć do części [Samouczek 1 — tworzenie obrazów kontenera][aks-tutorial-prepare-app].

Ten samouczek wymaga interfejsu wiersza polecenia platformy Azure w wersji 2.0.44 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli-install].

## <a name="update-an-application"></a>Aktualizowanie aplikacji

Wprowadźmy zmianę w przykładowej aplikacji, a następnie zaktualizujmy wersję już wdrożoną do klastra usługi AKS. Kod źródłowy aplikacji przykładowej znajduje się w katalogu *azure-vote*. Otwórz plik *config_file.cfg* za pomocą edytora, takiego jak `vi`:

```console
vi azure-vote/azure-vote/config_file.cfg
```

Zmień wartości parametrów *VOTE1VALUE* i *VOTE2VALUE* na różne kolory. Poniższy przykład przedstawia zaktualizowane wartości kolorów:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Zapisz i zamknij plik.

## <a name="update-the-container-image"></a>Aktualizowanie obrazu kontenera

Użyj narzędzia [docker-compose][docker-compose], aby ponownie utworzyć obraz frontonu i przetestować zaktualizowaną aplikację. Argument `--build` jest używany w celu poinstruowania narzędzia Docker Compose o konieczności ponownego utworzenia obrazu aplikacji:

```console
docker-compose up --build -d
```

## <a name="test-the-application-locally"></a>Testowanie aplikacji w środowisku lokalnym

Aby sprawdzić, czy w zaktualizowanym obrazie kontenera wyświetlane są wprowadzone zmiany, otwórz stronę http://localhost:8080 w lokalnej przeglądarce internetowej.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

Zaktualizowane wartości kolorów w pliku *config_file.cfg* są wyświetlane w uruchomionej aplikacji.

## <a name="tag-and-push-the-image"></a>Tagowanie i wypychanie obrazu

Aby w sposób prawidłowy użyć zaktualizowanego obrazu, otaguj obraz *azure-vote-front* za pomocą nazwy serwera logowania usługi ACR. Nazwę serwera logowania uzyskaj przy użyciu polecenia [az acr list](/cli/azure/acr#az_acr_list):

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Użyj polecenia [docker tag][docker-tag] w celu otagowania obrazu. Zastąp wartość `<acrLoginServer>` nazwą serwera logowania usługi ACR lub nazwą hosta rejestru publicznego i zaktualizuj wersję obrazu do wartości *:v2* w następujący sposób:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v2
```

Następnie użyj polecenia [docker push][docker-push] w celu przekazania obrazu do rejestru. Zastąp wartość `<acrLoginServer>` nazwą serwera logowania usługi ACR. Jeśli wystąpią problemy związane z wypychaniem do rejestru usługi ACR, upewnij się, że uruchomiono polecenie [az acr login][az-acr-login].

```console
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-the-updated-application"></a>Wdrażanie zaktualizowanej aplikacji

Aby zapewnić maksymalny czas działania, należy uruchomić wiele wystąpień zasobnika aplikacji. Sprawdź liczbę uruchomionych wystąpień frontonu, używając polecenia [kubectl get pods][kubectl-get]:

```
$ kubectl get pods

NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Jeśli nie masz wielu zasobników frontonu, przeskaluj wdrożenie *azure-vote-front* w następujący sposób:

```console
kubectl scale --replicas=3 deployment/azure-vote-front
```

Aby zaktualizować aplikację, użyj polecenia [kubectl set][kubectl-set]. Zaktualizuj wartość `<acrLoginServer>` nazwą serwera logowania lub nazwą hosta rejestru kontenerów, a następnie określ wersję aplikacji *v2*:

```console
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Aby monitorować wdrożenie, użyj polecenia [kubectl get pod][kubectl-get]. Podczas wdrażania aplikacji działanie zasobników jest przerywane, a następnie są one ponownie tworzone przy użyciu obrazu kontenera.

```console
kubectl get pods
```

Poniższe przykładowe dane wyjściowe przedstawiają zasobniki kończące swoje działanie oraz nowe wystąpienia uruchamiane w miarę postępów wdrażania:

```
$ kubectl get pods

NAME                               READY     STATUS        RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running       0          5m
azure-vote-front-1297194256-tpjlg  1/1       Running       0          1m
azure-vote-front-1297194256-tptnx  1/1       Running       0          5m
azure-vote-front-1297194256-zktw9  1/1       Terminating   0          1m
```

## <a name="test-the-updated-application"></a>Testowanie zaktualizowanej aplikacji

Aby wyświetlić zaktualizowaną aplikację, najpierw uzyskaj zewnętrzny adres IP usługi `azure-vote-front`:

```console
kubectl get service azure-vote-front
```

Następnie otwórz adres IP w lokalnej przeglądarce internetowej.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zaktualizowano aplikację i wydano tę aktualizację do klastra Kubernetes. W tym samouczku omówiono:

> [!div class="checklist"]
> * Aktualizowanie kodu aplikacji frontonu
> * Tworzenie zaktualizowanego obrazu kontenera
> * Wypychanie obrazu kontenera do usługi Azure Container Registry
> * Wdrażanie zaktualizowanego obrazu kontenera

Przejdź do następnego samouczka, aby dowiedzieć się, jak uaktualnić klaster AKS do nowej wersji klastra Kubernetes.

> [!div class="nextstepaction"]
> [Upgrade Kubernetes (Uaktualnianie usługi Kubernetes)][aks-tutorial-upgrade]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[az-acr-login]: /cli/azure/acr#az_acr_login
[azure-cli-install]: /cli/azure/install-azure-cli
