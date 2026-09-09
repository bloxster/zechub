<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Nœuds Lightwallet Zcash

## TL;DR

* La plupart des gens utilisent Zcash via un portefeuille léger, qui ne télécharge pas toute la blockchain. Il communique plutôt avec un serveur ayant déjà effectué ce travail.
* Deux logiciels servent aujourd’hui les portefeuilles légers : **lightwalletd**, le service original écrit en Go, et **Zaino**, un indexeur plus récent écrit en Rust.
* Vos clés ne quittent jamais votre appareil, et le serveur ne peut ni dépenser vos fonds ni lire les montants et mémos des transactions entièrement protégées.
* Le serveur est bien placé pour connaître votre adresse IP et le moment de votre activité — les transactions protégées préservent ce qui se passe sur la blockchain, pas votre connexion au serveur.
* Tor supprime l’identifiant IP ; il est disponible dans les portefeuilles construits sur `zcash_client_backend`, et dans ZODL il s’agit d’un réglage dans les Paramètres avancés.
* Vous pouvez changer le serveur utilisé par votre portefeuille ou exécuter le vôtre — lightwalletd et Zaino sont tous deux open source.

## Explication centrale

La plupart des gens utilisent Zcash via un portefeuille léger, qui ne télécharge pas toute la blockchain. Il communique plutôt avec un serveur ayant déjà effectué ce travail. Cette page explique ce que sont ces serveurs, ce qu’ils peuvent et ne peuvent pas voir à votre sujet, comment faire passer votre connexion par Tor et comment changer le serveur utilisé par votre portefeuille.

Deux logiciels servent aujourd’hui les portefeuilles légers. **lightwalletd** est le service original, écrit en Go. **Zaino** est un indexeur plus récent écrit en Rust, conçu dans le cadre de l’abandon de zcashd.

### Ce que fait un serveur de portefeuille léger

Un serveur de portefeuille léger se place entre votre portefeuille et la blockchain Zcash et lui fournit une vue de la chaîne efficace en bande passante. Il accomplit trois choses pour vous.

Il fournit des blocs compacts. Au lieu de blocs entiers, il envoie une forme compacte contenant uniquement ce dont un portefeuille a besoin pour détecter un paiement vers son adresse protégée, détecter une dépense de ses notes et mettre à jour ses témoins.

Il relaie vos transactions. Lorsque vous envoyez des fonds, votre portefeuille transmet la transaction terminée au serveur, qui la diffuse sur le réseau.

Il répond aux requêtes sur la chaîne, comme la hauteur actuelle et les informations de frais dont votre portefeuille a besoin.

Votre portefeuille effectue toujours le travail privé localement. Il détient vos clés, déchiffre les blocs à l’essai pour trouver vos notes, et construit et signe les transactions sur votre appareil.

### Ce que le serveur peut et ne peut pas voir

C’est la partie qu’il est facile de mal comprendre. Vos clés ne quittent jamais votre appareil, mais cela ne signifie pas que le serveur ne sait rien de vous.

La référence ici est le [modèle de menace de l’application de portefeuille Zcash](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html), qui mérite d’être lu intégralement si ce sujet vous importe. Il présente plusieurs types d’adversaires. Celui qui compte pour cette page est un adversaire capable d’observer le trafic entre votre portefeuille et Internet, ainsi qu’entre le serveur et Internet. La personne qui exécute le serveur est intrinsèquement en partie dans cette position, car votre portefeuille se connecte directement à elle.

Commençons par ce qui est protégé. Face à tous les adversaires du modèle, y compris un adversaire ayant compromis le serveur, il « ne peut apprendre aucun élément du matériel cryptographique de l’utilisateur (clés de dépense, clés de visualisation, phrase de récupération, etc.) », ne peut pas voler vos fonds et ne peut pas vous faire envoyer des fonds que vous n’aviez pas l’intention d’envoyer. Les montants et mémos des transactions entièrement protégées restent chiffrés.

Il y a ensuite ce qui n’est pas protégé. Le modèle de menace présente les éléments suivants comme des faiblesses connues face à un adversaire observant le trafic :

| Faiblesse | Comment |
|:--|:--|
| Déterminer qui vous êtes | « L’adversaire connaît l’adresse IP de l’utilisateur, ce qui pourrait le conduire à son identité réelle » |
| Déterminer approximativement où vous êtes | Rechercher votre IP « dans une base de données de géolocalisation afin d’estimer approximativement sa position » |
| Déterminer si et quand vous avez envoyé ou reçu une transaction protégée | L’envoi « utilise davantage de bande passante, ce qui est visible même si la connexion est chiffrée ». Le modèle note que l’acte d’envoyer et de recevoir est visible par le serveur lui-même |
| Compter le nombre de transactions que vous avez effectuées au fil du temps | Les mêmes schémas de bande passante, observés sur une période plus longue |
| Repérer des schémas de paiement récurrents | Observer les moments où l’activité a lieu |
| Déterminer si une adresse est la vôtre | Un adversaire qui connaît déjà une adresse « pourrait envoyer des fonds à cette adresse et observer s’il y a des pics de bande passante » lorsque votre portefeuille la récupère |

Le modèle note également que le cas ordinaire suppose « une relation de confiance entre l’utilisateur et l’opérateur du serveur lightwalletd ».

Le résumé honnête est donc le suivant. Un serveur de portefeuille léger ne peut pas dépenser votre argent, et il ne peut pas lire les montants ou les mémos de vos transactions protégées. Ce qu’il est bien placé pour connaître est votre adresse IP et le moment de votre activité, et ces deux éléments réunis peuvent en dire long sur une personne. Les transactions protégées préservent ce qui se passe sur la blockchain. Elles ne cachent pas, à elles seules, votre connexion au serveur.

## Visuel / Analogie

Imaginez une bibliothèque publique qui détient tous les journaux jamais imprimés. Un nœud complet est un lecteur qui emporte chez lui l’intégralité des archives. Un portefeuille léger est un lecteur qui demande plutôt au bibliothécaire un condensé quotidien — une fine feuille contenant juste assez d’informations pour repérer si quelque chose le concerne.

Le condensé est scellé : le bibliothécaire le prépare sans pouvoir lire quels éléments comptent pour vous, et vous l’ouvrez chez vous avec votre propre clé. C’est le bloc compact, et l’ouverture est le déchiffrement à l’essai sur votre appareil.

Mais le bibliothécaire voit toujours quel lecteur entre, à quel moment et l’épaisseur du paquet qu’il emporte. C’est l’adresse IP et le moment de l’activité — visibles depuis le comptoir, quelle que soit la qualité du scellement de l’enveloppe. Tor équivaut à envoyer un coursier anonyme : le bibliothécaire remet toujours le même paquet, mais ne sait plus à quelle maison il est destiné.

## Approfondissement

### Routage par Tor

Tor rompt le lien entre votre adresse IP et le trafic de votre portefeuille, ce qui supprime l’identifiant le plus fort du tableau ci-dessus.

La prise en charge existe dans les bibliothèques Rust sur lesquelles sont construits de nombreux portefeuilles Zcash. zcash_client_backend comprend un module Tor basé sur [Arti](https://tpo.pages.torproject.net/core/arti/), l’implémentation Rust de Tor ; un portefeuille peut donc faire passer la synchronisation, la diffusion des transactions et les recherches de prix par Tor sans inclure un client Tor distinct.

Les développeurs de Zaino avancent le même argument, en citant directement le modèle de menace : il existe « un besoin d’utiliser des protocoles de transport anonymes (tels que Nym ou Tor) afin d’obscurcir l’identité des clients auprès des serveurs d’indexation de Zcash ».

Dans **ZODL**, Tor est un réglage dans les Paramètres avancés. Les notes de version du portefeuille conseillent aux utilisateurs le mode de connexion manuelle « ainsi que l’activation de Tor dans les Paramètres avancés » s’ils « préfèrent réduire l’exposition des métadonnées », et l’application propose d’activer Tor avant la restauration d’un portefeuille, le moment où une nouvelle IP serait autrement liée à tout l’historique d’un portefeuille.

Deux réserves. Tor masque votre IP au serveur, mais ne change pas ce que le serveur apprend des requêtes que vous effectuez. Et le routage en oignon ajoute de la latence, donc la synchronisation prend plus de temps. Exécuter votre propre serveur évite différemment la question de confiance, puisque vous en êtes alors l’opérateur.

### Zaino, l’indexeur Rust

[Zaino](/zcash-tech/zaino) est un indexeur écrit en Rust par l’équipe Zingo, conçu pour remplacer lightwalletd dans le cadre de l’abandon de zcashd. Il sert les clients légers, les clients complets et les explorateurs de blocs, en lisant les données de chaîne détenues par « un validateur complet Zebra ou Zcashd ».

Il est en développement actif, la version 0.8.0 ayant été publiée en août 2026. Son objectif est de rester rétrocompatible avec lightwalletd lorsque cela est possible, afin que les portefeuilles puissent le cibler sans être réécrits.

Zaino dispose de sa propre page avec des diagrammes d’architecture ; cette page ne couvre donc que son rôle de serveur de portefeuille léger.

### Exécuter le vôtre

L’option la plus solide est d’être votre propre opérateur, ce qui supprime entièrement la question de confiance. Les deux serveurs sont open source : [lightwalletd](https://github.com/zcash/lightwalletd) en Go et [Zaino](https://github.com/zingolabs/zaino) en Rust. Tous deux lisent depuis un validateur complet, vous voudrez donc aussi [Zebra](/zcash-tech/zebra-full-node).

## Implications pratiques

### Liste des serveurs

Le tableau de bord [hosh.zec.rocks](https://hosh.zec.rocks/zec) suit les serveurs publics et leur état de santé, et c’est l’endroit où vérifier ce qui est réellement disponible. [status.zec.rocks](https://status.zec.rocks/) affiche l’état des services.

Serveurs répertoriés sur ce tableau de bord au moment de la rédaction :

| Serveur | Notes |
|:--|:--|
| zec.rocks:443 | Des points de terminaison régionaux sont répertoriés à ses côtés : na.zec.rocks, eu.zec.rocks, ap.zec.rocks et sa.zec.rocks |
| zec-node.cakewallet.com:443 | Sur le domaine de Cake Wallet |
| zec.0xrpc.io:443 | Exécuté par 0xRPC, qui propose des points de terminaison publics gratuits pour plusieurs chaînes et demande des dons pour couvrir la capacité |
| zaino.unsafe.zec.rocks:443 | Une instance Zaino. Prenez note du nom d’hôte et considérez-la comme expérimentale |
| testnet.zec.rocks:443 | Testnet, avec une instance Zaino testnet répertoriée à zaino.testnet.unsafe.zec.rocks |

Consultez le tableau de bord plutôt que de vous fier à cette liste. Les opérateurs vont et viennent, et une page comme celle-ci vieillit.

### Changer le serveur dans votre portefeuille

Cela vaut la peine si vous souhaitez choisir un opérateur auquel vous faites confiance, répartir l’activité entre plusieurs opérateurs ou cibler le vôtre.

Les chemins de menu ci-dessous étaient corrects lors de la mise à jour de cette page, mais les interfaces de portefeuille évoluent ; considérez-les donc comme une indication plutôt qu’un itinéraire exact. Recherchez les Paramètres avancés ou une option de serveur.

#### ZODL

Anciennement Zashi. L’icône d’engrenage dans le coin supérieur droit, puis Paramètres avancés. Tor se trouve sur le même écran. ZODL propose aussi un raccourci Switch server lorsqu’un échec de synchronisation est dû à un serveur obsolète.

#### Ywallet

L’icône d’engrenage dans le coin supérieur droit, puis l’onglet Zcash.

![Paramètres de serveur Ywallet](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

Le menu hamburger dans le coin supérieur gauche, puis Settings, puis faites défiler vers le bas.

![Paramètres de serveur Zingo](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

Le menu hamburger dans le coin supérieur gauche, puis Settings, puis Advanced.

![Paramètres de serveur eZcash](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

Ces captures d’écran ont été prises en mars 2025, et les applications ont publié des versions depuis ; les boutons ont donc pu être déplacés.

## Erreurs courantes

**Penser que le serveur peut lire vos transactions**. Il ne le peut pas. Vos clés restent sur votre appareil, et les montants et mémos des transactions entièrement protégées restent chiffrés — même face à un adversaire ayant compromis le serveur.

**Interpréter « protected » comme « connexion anonyme »**. Les transactions protégées préservent ce qui se passe sur la blockchain. Votre adresse IP et le moment de votre activité constituent une couche distincte, et c’est précisément cette couche que le serveur voit.

**Supposer que Tor supprime toute trace**. Tor masque votre IP au serveur, mais ne change pas ce que le serveur apprend des requêtes que vous effectuez, et il ajoute de la latence à la synchronisation.

**Faire confiance à une liste de serveurs sur une page wiki**. Les opérateurs vont et viennent. Consultez [hosh.zec.rocks](https://hosh.zec.rocks/zec) pour voir ce qui fonctionne réellement avant de diriger votre portefeuille vers quoi que ce soit.

## Résumé

Les portefeuilles légers vous donnent accès au pool protégé sans l’espace disque, ce qui est un bon compromis. Soyez simplement au clair sur ce que vous échangez. Le serveur ne peut ni prendre vos fonds ni lire vos montants protégés, mais il est bien placé pour voir votre adresse IP et quand vous effectuez des transactions. Passez par Tor, choisissez votre opérateur délibérément ou exécutez le vôtre.

## Pages associées

- [Qui peut voir votre paiement Zcash](/start-here/who-can-see-your-zcash-payment) — une vue de la même question destinée aux débutants.
- [Ce qu’un explorateur de blocs peut voir](/zcash-tech/what-a-block-explorer-can-see) — ce qui est visible on-chain, par opposition à ce qui est visible au serveur.
- [Zaino](/zcash-tech/zaino) — diagrammes d’architecture et rôle plus large de l’indexeur Rust.
- [Nœud complet Zebra](/zcash-tech/zebra-full-node) — le validateur depuis lequel lit un serveur de portefeuille léger.
- [Synchronisation des portefeuilles Zcash](/zcash-tech/zcash-wallet-syncing) — comment les blocs compacts envoyés par un serveur sont traités par votre portefeuille.

**Dernière mise à jour :** août 2026
