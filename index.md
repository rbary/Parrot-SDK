## Parrot SDK: Prise en main et possibilités d'utilisation

ARDroneSDK3 est à ce jour la dernière version du Parrot SDK (version 3.8), publiée en novembre 2014. Pour faire simple, ce SDK permet de contrôler la plupart des drones et/ou minidrones de Parrot (Rolling Spider, Bebop Drone, Skycontroller, Jumping Sumo).
Il faut entendre par "contrôler" le fait d'effectuer les actions suivantes : 
+ Se connecter à un drone
+ Piloter un drone
+ Recevoir du stream depuis un drone
+ Sauvegarder et télécharger des photos ou des vidéos réalisées par un drone
+ Transmettre et faire jouer des séquences automatiques de vol (autopilot flight plan)
+ Mettre à jour le drone
+ Faire du mapping 3D


![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/jumpingsumorollingspider.jpg)

**Exemple d'utilisation du Parrot SDK**: l'application FreeFlight3 qu'il est recommandé d'installer sous Android ou iOS lors de l'achat d'un drone ou minidrone, utilise l'ARDroneSDK3.


![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/freeflight.jpg)

>Est-il possible de créer sa propre application de contrôle de drones avec ce SDK ?"

Oui, tout à fait, c'est tout l'intérêt de la mise à disposition des sources du SDK. Si vous avez des compétences en développement d'applications mobiles (Android ou iOS), il vous suffira de créer un projet de développement mobile (Android Studio, Plugin Eclipse ADT, XCode), ensuite de mettre en place les dépendances nécessaires pour avoir la main sur les bibliothèques du SDK et enfin les utiliser à votre escient dans le code de votre application mobile.
Il est aussi possible d'implémenter une application de type client lourd en C permettant de contrôler le drone depuis un laptop (ici sous xubuntu 14.04) 

En effet, les sources du SDK sont hébergés dans un répertoire Github sous licence BSD. Ce répertoire contient principalement : 
+ Les sources des différentes bibliothèques qui constituent le SDK
+ Des utilitaires de Build
+ Un manifest
+ Quelques exemples d'utilisation du SDK pour chaque plateforme (Unix, Android, iOS)

On peut retrouver l'organisation complète du SDK par [ici](http://developer.parrot.com/docs/bebop/?c#organisation)

## Comment utiliser le SDK ?

Le SDK est principalement écrit en C, et fourni donc des bibliothèques pour les Systèmes Unix, Android et iOS respectivement utilisables en C, Java et Objective C.

**NB**: Les manipulations pour ce billet concernent les plateformes Unix et Android.

### Outils pré-requis
Il faut commencer par installer au préalable les outils suivants:
+ [curl](#curl)
+ [repo](#repo)
+ [git](#git)
+ [automake](#automake)
+ [libtool](#libtool)
+ [yasm](#yasm)
+ [nasm](#nasm)

<a name="curl"></a>
#### Installer curl
Dans un terminal, Faire `$ sudo apt-get install curl`

<a name="repo"></a>
#### Installer Repo tool

```
$ mkdir ~/bin
$ PATH=~/bin:$PATH
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
```

#### Installer git
<a name="git"></a>
Faire `$ sudo apt-get insall git-core`
Ensuite effectuer la config minimale

```
$ git config --global user.name "Your Name"
$ git config --global user.email "jhon@doe.com"
```

<a name="automake"></a>
#### Installer automake

Automake étant inclu dans autoconf, on installe simplement autoconf

`$ sudo apt-get install autoconf`


<a name="libtool"></a>
#### Installer libtool

`$ sudo apt-get install libtool`

<a name="yasm"></a>
#### Installer yasm

`$ sudo apt-get install yasm`

<a name="nasm"></a>
#### Installer nasm

`$ sudo apt-get install nasm`

### Étape 1: Récupération des sources avec Repo
1.Créer un dossier vide dans lequel on va mettre en place notre SDK
```
$ mkdir ~/ARDroneSDK
$ cd ~/ARDroneSDK
```
2.Lancer la commande `repo init` pour initialiser un repo avec le manifest du SDK

`$ repo init -u https://github.com/Parrot-Developers/arsdk_manifests.git`

3.Descendre l'arbre complet des sources distants du SDK

`$ repo sync`

### Étape 2: Le Build
#### Pour une plateforme Unix
Les manipulation pour un build sous Unix sont les suivantes :

```
$ cd ~/ARDroneSDK
$ ./build.sh -p Unix-forall -t build-sdk-j
```

On aura en sortie du build le répertoire `~/ARDroneSDK/out/Unix-base/usr`.
On peut à présent faire fonctionner les exemples sous Unix fournis avec le SDK qu'on peut trouver dans le répertoire `~/ARDroneSDK/packages/Samples/Unix`.

#### Pour une plateforme Android

Pour un build Android (IDE Android sur Unix) il faut :
+ un JDK avec Java 6(1.6) minimum
+ [Android SDK](http://developer.android.com/sdk/installing/index.html) et [Android NDK](http://developer.android.com/ndk/index.html)
+ Déclarer les variables d'environnement `ANDROID_SDK_PATH` et 
`ANDROID_NDK_PATH` pour pointer sur les répertoires correspondants aux outils respectifs.

Nous utiliserons [Android Studio](http://developer.android.com/sdk/index.html#top)) pour notre exemple. C'est un bundle avec lequel on peut avoir de base une installation de Android SDK.

##### Installer Android Studio et Android SDK
Télécharger [Android Studio](http://developer.android.com/sdk/index.html#Other) à la racine de votre répertoire personnel. Dézippez `android-studio-ide-141.2456560-linux.zip` puis dans un terminal, faites :
```
$ cd ~/android-studio/bin/
$ ./studio.sh
```
Android studio vous indiquera qu'il vous manque Android SDK. Faites `next` pour l'installer.

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/install_android_sdk.png)

Vous pouvez choisir l'emplacement qui vous convient; Notons qu'il est préférable de l'installer à l'extérieur de android-studio
Puis vous faites `finish`.

##### Installer Android NDK
Android NDK est un ensemble d'outils permettant d'implémenter une partie d'une application avec du code natif C ou C++. Nous installerons Android NDK à la racine de notre répertoire personnel.

Commencez par télécharger le paquet correspondant depuis ce [lien](http://developer.android.com/ndk/downloads/index.html) et suivez les instructions d'installation

##### Déclarer les variables d'environnement

Android studio sera prêt à être utilisé, mais vous pouvez fermer la fenêtre.
On a donc Android SDK et Android NDK installés respectivement aux emplacements 
`/home/rbary/Android/SDK` et 
`/home/rbary/android-ndk-r10e`.
On renseigne les variables d'environnement nécessaires (`ANDROID_SDK_PATH` et 
`ANDROID_NDK_PATH`) avec ces chemins dans le fichier /etc/environnement en root comme ceci:

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/android_sdk_ndk_path.png)

Ensuite `$ source /etc/environnement`.

Puis on vérifie, qu'elles ont été déclarées:

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/android_sdk_ndk_path_check.png)

On peut dès lors builder notre Parrot SDK pour Android. On refait les mêmes manipulations de l'étape 1 en donnant un autre nom au répertoire qui va recevoir le SDK. `ARDroneSDK-Android` par exemple.

```
$ mkdir ~/ARDroneSDK-Android
$ cd ~/ARDroneSDK-Android
$ repo init -u https://github.com/Parrot-Developers/arsdk_manifests.git
$ repo sync
```
Pour préparer la mise en marche de notre example d'utilisation du SDK sous Android nous allons en plus du build des bibliothèques du SDK, construire les dépendances également pour les examples (Build JNI, Exécution Gradlew assembledebug pour Android Studio). Donc, à la racine de `~ARDroneSDK-Android` nous ferons 

```
$ ./build.sh -p Android-forall -t build-sample -j
```
Au lieu de faire 
```
$ ./build.sh -p Android-forall -t build-sdk -j
```


Vous pouvez aller vous prendre un café car le build pour Android prend une bonne dizaine de minutes ...

Ce build nous donnera en sortie plusieurs répertoires 
`~/ARDroneSDK-Android/out/Android-[VARIANTE]/staging/usr`

Les valeurs possibles pour [VARIANTE] étant:
+ armeabi
+ armeabi_v7a
+ mips
+ x86


### Étape 3: Utilisation du SDK
#### Exemple d'utilisation sous Unix : *JumpingSumoPiloting*

Nous nous contenterons ici, pour le moment, de faire fonctionner l'un des exemples d'utilisation livré avec le SDK, en l’occurrence 
*JumpingSumoPiloting*.

##### Mise en place des dépendances

A ce jour, les instructions décrites sur le [site des développeurs Parrot](http://developer.parrot.com/docs/bebop/?c#general-build) pour faire fonctionner les exemples du SDK, sont sans issue, du moins pour Unix.
Les "Makefiles" des exemples sous Unix n'étant pas à jour par rapport à l'architecture du SDK, il faudra y effectuer quelques modifications, et faire  ensuite quelques builds atomiques pour les bibliothèques manquantes.
Focalisons nous à présent sur les manipulations à faire pour l'exemple "Jumping Sumo Piloting".

Avec votre éditeur de texte favoris, ouvrez le Makefile situé à cet emplacement
`~/ARDroneSDK/packages/Samples/Unix/JumpingSumoPiloting`, puis faites les modifications suivantes:

```
SDK_DIR=~/ARDroneSDK

CFLAGS=-I$(IDIR) -I $(SDK_DIR)/out/Unix-base/staging/usr/include

LIBS=-L$(SDK_DIR)/out/Unix-base/staging/usr/lib -larsal -larcommands -larnetwork
 -larnetworkal -lardiscovery $(EXTERNAL_LIB)

LIBS_DBG=-L$(SDK_DIR)/out/Unix-base/staging/usr/lib -larsal_dbg -larcommands_dbg
 -larnetwork_dbg -larnetworkal_dbg -lardiscovery_dbg $(EXTERNAL_LIB)
```

Vous remarquerez dans les lignes précédentes du Makefile que, 
*JumpingSumoPiloting* aura besoin des bibliothèques `libarsal`,`libarcommands`,
`libarnetwork`,`libarnetworkal` et `libardiscovery` pour fonctionner.

*Problème* : La bibliothèque `libardiscovery` est manquante dans le répertoire 
`~/ARDroneSDK/out/Unix-base/staging/usr/lib` qui contient l'ensemble des bibliothèques qui ont été construites pour la plateforme Unix lors du Build complet du SDK.

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/libardiscovery_miss.png)

*Solution* : Faire un build élémentaire pour cette bibliothèque.

Placez vous dans le répertoire `~/ARDroneSDK/package/libARDiscovery` qui contient les sources de notre bibliothèque manquante puis vous faites :

```
$ cd Build
$ ./bootstrap
$ ./configure --prefix=/home/rbary/ARDroneSDK/out/Unix-base/staging/usr 
--with-libARSALInclude=~/ARDroneSDK/out/Unix-base/staging/usr/include
$ make
$ make install
```

Il faut renseigner pour l'argument `--prefix` l'emplacement où l'on souhaite installer la bibliothèque. Attention il faut donner un chemin absolu comme nous l'avons fait. On peut voir que notre bibliothèque dépend des includes (headers) de `libARSAL` situés à cet emplacement 
`~/ARDroneSDK/out/Unix-base/staging/usr/include`

Le résultat attendu est le suivant :

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/libardiscovery_lib_include.png)

`libardiscovery` est bien installé dans le répertoire 
`~/ARDroneSDK/out/Unix-base/staging/usr/lib` et les headers correspondants (qui pourront être utilisés par d'autres bibliothèques) le sont aussi dans le répertoire `~/ARDroneSDK/out/Unix-base/staging/usr/include`

Remarque : Le répertoire `~/ARDroneSDK/package/` en plus de contenir des exemples de fonctionnement du SDK, comprend également les sources de toutes les bibliothèques du SDK. S'il vous manque, une bibliothèque pour un exemple donné il vous suffira de refaire la manipulation précédente pour la bibliothèque concernée.

On peut remarquer que dans le Makefile, que notre exemple dépend d'une bibliothèque externe, permettant de disposer d'une interface dans un terminal texte et aussi d'utiliser la souris ainsi que les touches de fonctions du clavier : [`libncurses` ](http://arnaud-feltz.developpez.com/tutoriels/ncurses/?page=introduction#LI-A)

![lncurses](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/lncurses.png)

On installe libncurses si nécessaire:
```
$ sudo apt-get update
$ sudo apt-get install ncurses-dev
```
##### Mise en marche

Notre SDK étant installé correctement, les problèmes de dépendances ayant été résolus, on peut à présent faire tourner notre example *JumpingSumoPiloting*.

On compile les sources de *JumpingSumoPiloting* et on lance notre exécutable :

```
$ cd ~/ARDroneSDK/package/Samples/Unix/JumpingSumoPiloting
$ make
$ ./JumpingSumoPiloting
```

Si après le `make` vous avez une erreur comme sur l'image qui suit :

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/usrbinlink.png)

Il faudra créer un lien symbolique dans `/usr/lib` qui pointera vers chacune des bibliothèques nécessaires pour *JumpingSumoPiloting* comme ceci:

```
sudo ln -s ~/ARDroneSDK/out/Unix-base/staging/usr/lib/[nom_de_lib].so 
/usr/lib/[nom_de_lib].so
```

On a pour `libarsal` la commande suivante:

```
sudo ln -s ~/ARDroneSDK/out/Unix-base/staging/usr/lib/libarsal.so 
/usr/lib/libarsal.so
```

Exceptionellement, pour `libardiscovery` la commande sera:

```
sudo ln -s ~/ARDroneSDK/out/Unix-base/staging/usr/lib/libardiscovery-3.1.0.so 
/usr/lib/libardiscovery-3.1.0.so
```

>Que fait JumpingSumoPiloting ?

*JumpingSumoPiloting* est un exemple simple d'utilisation du Parrot SDK qui permet de réaliser les actions suivantes:

+ rechercher un mini drone aux alentours (notre Jumping Sumo)
+ se connecter au minidrone (ici en wifi)
+ envoyer et recevoir des commandes pour piloter le minidrone (Le pilotage du drone se fait avec les touches du clavier : touches directionnelles et Espace)
+ afficher l'état de la batterie du minidrone dans une IHM

Le tableau qui suit fait un récapitulatif des services fournis par les différentes bibliothèques du SDK qui sont utilisées par *JumpingSumoPiloting*

| bibliothèques     | Services          |
| ---------------| ------------------- 
| libARSAL       | Une couche d'abstraction du système |  
| libARCommands  | Les commandes qu'on peut envoyer et recevoir au/du drone| 
| libARNetwork   | L'envoi et la réception des paquets au/depuis le drone  | 
| libARNetworkal | Un couche d'abstraction pour les différents types de réseau sans-fil (Bluetooth Low Energy ou Wifi)|
| libARDiscovery | La découverte dans le réseau des drones supportés|

[Pour en savoir plus sur les autres bibliothèques ](http://developer.parrot.com/docs/bebop/#organisation)

[Pour connaître les caractéristiques techniques du Jumping Sumo](http://www.parrot.com/fr/produits/jumping-sumo/)

#### Exemple d'utilisation sous Android : *RollingSpiderPiloting*

Par analogie à l'exemple sous Unix, nous nous focaliserons sur 
*RollingSpiderPiloting*. C'est un exemple également basique d'utilisation du Parrot SDK dans un projet de développement d'application Android. 

##### Mise en place et mise en fonctionnement

Commençons par lancer Android Studio avec `$ ~/android-studio/bin/studio.sh` et importez notre exemple comme ceci:
![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/open_existing_project.png)

Ouvrez l'explorateur projet, on voit bien que les dépendences aux librairies du SDK Parrot sont effectivement établies. Ouvrez le fichier 
`build.gradle (Module: app)` puis dans `dependencies { }` remplacez les lignes
suivantes 

```
    compile project(':libARCommands')
    compile project(':libARDiscovery')
    compile project(':libARNetwork')
    compile project(':libARNetworkAL')
    compile project(':libARSAL')
```
par `compile 'com.parrot:arsdk:3.8.3'`

Dans `defaultConfig{ ...}` rajoutez la ligne `multiDexEnabled true` pour activer le support MultiDex ![explication]() et mettez à jour 
`build.gradle` avec `Sync now`

![](https://raw.githubusercontent.com/rbary/ParrotSDK_Undergrowth/gh-pages/images/parrot/buildgradle_sync.png)

Editez le fichier `MainActivity.java` qui se trouve dans 
`app > src > main > java > com > parrot > rollingspiderpiloting` en modifiant les lignes **87** et **285** par:
```
ArrayAdapter<String> adapter = new ArrayAdapter<String>(MainActivity.this, android.R.layout.simple_list_item_1, android.R.id.text1, deviceNameList);
```
Ensuite vous faites `Build > Clean Project`.

On peut à présent faire `Run` et choisir son appareil de déploiement (Tablette/Smartphone Android ou Appareil Virtuel de l'emulateur).


#### Focus sur **RSPilotingMin** et **MyJSPilotingMin**:
MyRSPiloting et MyJSPiloting sont deux implémentations basiques (sans IHM) que nous réalisons pour nous focaliser sur les commandes, essentielles pour contrôler nos minidrones.

##### **RSPilotingMin**
En cours d'implémentation pour des aspects de connectivité *Bluethooh*

##### **JSPilotingMin**
Dans cette implémentation, nous nous sommes donc focalisés sur un ensemble minimaliste de commandes, nécessaires pour le contrôle d'un appareil de type 
**Jumping Sumo**, permettant ainsi de réaliser les taĉhes suivantes :

+ Créer un `Discovery Device`
+ Initialiser le `Discovery Device` avec un `Wifi Device`
+ Créer un `Device Controller` avec le `Discovery Device`
+ Supprimer le `Discovery Device`
+ Ajouter une fonction de callback à au `Device Controller`  (on peut le voir comme un listener sur le deviceController) permettant notifier les changements d'état du `Device Controller` (les états possibles ici étant "en marche", "en arrêt") 
+ Ajouter au `Device Controller ` une fonction de callback permettant une notification quand on reçoit une commande venant de l'appareil.
+ Démarrer le `Device Controller`, impliquant une connection wifi entre le drone et notre système Unix.
+ Récupérer l'état courant du `Device Controller`
+ Arrêter de `Device Controller`, impliquant une déconnection wifi entre le drone et notre système Unix.
+ Envoyer les commandes de pilotage suivantes à l'appareil, avec le 
`Device Controller`:
    * Sauter
    * Avancer
    * Reculer
    * Tourner à droite
    * Tourner à gauche

Le tableau suivant récapitule les commandes en C du SDK qui permettent de réaliser les services énumérées en amont et les librairies concernées.

| Services |Librairie du SDK Concernée | Commande associée en C |
|----------|---------------------------|---------------------|
| Créer un Discovery Device| libARDiscovery |` ARDISCOVERY_Device_t *ARDISCOVERY_Device_New (eARDISCOVERY_ERROR *error);`|
| Initaliser un Discovery Device avec un Wifi Device| libARDiscovery |`eARDISCOVERY_ERROR ARDISCOVERY_Device_InitWifi (ARDISCOVERY_Device_t *device, eARDISCOVERY_PRODUCT product, const char *name,const char *address,int port);`|
| Créer un Device Controller| libARController |`ARCONTROLLER_Device_t *ARCONTROLLER_Device_New (ARDISCOVERY_Device_t *discoveryDevice, eARCONTROLLER_ERROR *error);`|
| Supprimer un Discovery Device| libARDiscovery |`void ARDISCOVERY_Device_Delete (ARDISCOVERY_Device_t **device);`|
| Ajouter une fonction de callback à un Device Controller permettant de notifier les changements d'état du Device Controller| libARController | `eARCONTROLLER_ERROR ARCONTROLLER_Device_AddCommandReceivedCallback (ARCONTROLLER_Device_t *deviceController, ARCONTROLLER_DICTIONARY_CALLBACK_t commandReceivedCallback, void *customData);`|
|Ajouter une fonction de callback à un Device Controller permettant une notification quand on reçoit une commande venant d'un appareil| libARController | `eARCONTROLLER_ERROR ARCONTROLLER_Device_AddStateChangedCallback (ARCONTROLLER_Device_t *deviceController, ARCONTROLLER_Device_StateChangedCallback_t stateChangedCallback, void *customData);`|
|Démarrer un Device Controller | libARController | `eARCONTROLLER_ERROR ARCONTROLLER_Device_Start (ARCONTROLLER_Device_t *deviceController);`|
|Arrêter un Device Controller | libARController | `eARCONTROLLER_ERROR ARCONTROLLER_Device_Start (ARCONTROLLER_Device_t *deviceController);`
|Récupérer l'état d'un Device Controller | libARController | `eARCONTROLLER_DEVICE_STATE ARCONTROLLER_Device_GetState (ARCONTROLLER_Device_t *deviceController, eARCONTROLLER_ERROR *error);`|
|Envoyer une commande pour faire *Sauter* l'appareil| libARController |`deviceController->jumpingSumo->sendAnimationsJump(deviceController->jumpingSumo,ARCOMMANDS_JUMPINGSUMO_ANIMATIONS_JUMP_TYPE_HIGH);`|
| Envoyer une commande pour faire *Avancer* l'appareil | libARController |`deviceController->jumpingSumo->setPilotingPCMDFlag(deviceController->jumpingSumo, 1);deviceController->jumpingSumo->setPilotingPCMDSpeed (deviceController->jumpingSumo, 50);`|
|Envoyer une commande pour faire *Reculer* l'appareil | libARController |`deviceController->jumpingSumo->setPilotingPCMDFlag (deviceController->jumpingSumo, 1);deviceController->jumpingSumo->setPilotingPCMDSpeed (deviceController->jumpingSumo, -50);`|
|Envoyer une commande pour faire *Tourner à droite* l'appareil|libARController|`deviceController->jumpingSumo->setPilotingPCMDFlag (deviceController->jumpingSumo, 1);deviceController->jumpingSumo->setPilotingPCMDTurn (deviceController->jumpingSumo, 50);`|
|Envoyer une commande pour faire *Tourner à gauche* l'appareil|libARController|`deviceController->jumpingSumo->setPilotingPCMDFlag (deviceController->jumpingSumo, 1);deviceController->jumpingSumo->setPilotingPCMDTurn (deviceController->jumpingSumo, -50);`|




