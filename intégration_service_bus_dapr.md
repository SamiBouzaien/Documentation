# Intégration de Service bus avec dapr

## 1. Ajouter dapr au cluster

- Assurez-vous que Helm 3 est installé sur votre machine

  - télècharger [Helm.exe](https://github.com/helm/helm/releases)
  - Ajouter le chemin de Helm.exe dans les variables d'environnement

- Ajouter le référentiel Helm et mettre à jour

```bash
helm repo add dapr https://dapr.github.io/helm-charts/
helm repo update
# See which chart versions are available
helm search repo dapr --devel --versions

```

- Installez le graphique Dapr sur votre cluster dans l' espace de dapr-systemnoms.

```bash
helm upgrade --install dapr dapr/dapr \
--version=1.8 \
--namespace dapr-system \
--create-namespace \
--wait
```

## 2. Dapr : Modèle de publication et d'abonnement

![dapr pubsub](./images/images_dapr/pubsub%20dapr.png)

Le modèle de publication et d'abonnement (pub/sub) permet aux microservices de communiquer entre eux à l'aide de messages pour les architectures pilotées par les événements.

Le producteur, ou éditeur, écrit des messages sur un canal d'entrée et les envoie à un sujet, sans savoir quelle application les recevra.
Le consommateur, ou abonné, s'abonne au sujet et reçoit des messages d'un canal de sortie, sans savoir quel service a produit ces messages.
Un courtier de messages intermédiaire copie chaque message du canal d'entrée d'un éditeur vers un canal de sortie pour tous les abonnés intéressés par ce message. Ce modèle est particulièrement utile lorsque vous devez découpler les microservices les uns des autres.
L'API pub/sub dans Dapr fournit une API indépendante de la plate-forme pour envoyer et recevoir des messages.
L'API pub/sub dans Dapr offre au moins une garantie de livraison de message et s'intègre à divers courtiers de messages et systèmes de file d'attente.
Le courtier de messages spécifique utilisé par votre service est enfichable et configuré en tant que composant Dapr pub/sub au moment de l'exécution. Cela supprime la dépendance de votre service et rend votre service plus portable et plus flexible aux changements.
Lorsque vous utilisez pub/sub dans Dapr :

- Votre service effectue un appel réseau vers une API de bloc de construction pub/sub Dapr.
- Le bloc de construction pub/sub effectue des appels dans un composant Dapr pub/sub qui encapsule un courtier de messages spécifique.
- Pour recevoir des messages sur un sujet, Dapr s'abonne au composant pub/sub au nom de votre service avec un sujet et transmet les messages à un point de terminaison sur votre service lorsqu'ils arrivent.

## 3. Création de composant pubsub

- Pour le déployer dans un cluster Kubernetes, remplissez les détails de connexion des métadonnées du composant pub/sub dans le YAML ci-dessous:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: order-pub-sub
  namespace: default
spec:
  type: pubsub.azure.servicebus
  version: v1
  metadata:
    - name: connectionString # Required when not using Azure Authentication.
      value: "Endpoint=sb://{ServiceBusNamespace}.servicebus.windows.net/;SharedAccessKeyName={PolicyName};SharedAccessKey={Key};EntityPath={ServiceBus}"
scopes:
  - orderprocessing
  - checkout
```

- Enregistrez sous pubsub.yaml

- Exécutez

```bash
 kubectl apply -f pubsub.yaml
```

## 4. intégrer dapr client dans un projet .Net

Dapr propose deux méthodes par lesquelles vous pouvez vous abonner à des sujets :

- De manière déclarative, où les abonnements sont définis dans un fichier externe.
- Par programmation, où les abonnements sont définis dans le code utilisateur.

Créez un fichier nommé subscription.yaml et collez ce qui suit :

```yaml
apiVersion: dapr.io/v1alpha1
kind: Subscription
metadata:
  name: order-pub-sub
spec:
  topic: orders
  route: /checkout
  pubsubname: order-pub-sub
scopes:
  - orderprocessing
  - checkout
```

Vous trouverez ci-dessous des exemples de code qui exploitent les SDK Dapr pour vous abonner au sujet que vous avez défini dans subscription.yaml.

```csharp
//dependencies
using System.Collections.Generic;
using System.Threading.Tasks;
using System;
using Microsoft.AspNetCore.Mvc;
using Dapr;
using Dapr.Client;

//code
namespace CheckoutService.controller
{
    [ApiController]
    public class CheckoutServiceController : Controller
    {
         //Subscribe to a topic
        [Topic("order-pub-sub", "orders")]
        [HttpPost("checkout")]
        public void getCheckout([FromBody] int orderId)
        {
            Console.WriteLine("Subscriber received : " + orderId);
        }
    }
}
```

Vous trouverez ci-dessous des exemples de code qui exploitent les SDK Dapr pour publier un sujet.

```csharp
//dependencies
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Dapr.Client;
using Microsoft.AspNetCore.Mvc;
using System.Threading;

//code
namespace EventService
{
    class Program
    {
        static async Task Main(string[] args)
        {
           string PUBSUB_NAME = "order-pub-sub";
           string TOPIC_NAME = "orders";
           while(true) {
                System.Threading.Thread.Sleep(5000);
                Random random = new Random();
                int orderId = random.Next(1,1000);
                CancellationTokenSource source = new CancellationTokenSource();
                CancellationToken cancellationToken = source.Token;
                using var client = new DaprClientBuilder().Build();
                //Using Dapr SDK to publish a topic
                await client.PublishEventAsync(PUBSUB_NAME, TOPIC_NAME, orderId, cancellationToken);
                Console.WriteLine("Published data: " + orderId);
		        }
        }
    }
}
```
