# Code structure


	Connection to Internet

	AzureIoT_Init

	Set Up Network Credentials				(On initialise une structure contenant des informations d'identification pour le réseau)

	

			TLS_Socket_Connect
		
	
		AzureIoTProvisioningClient_Init
		
		AzureIoTProvisioningClient_SetSymetricKey

		do: AzureIoTProvisioningClient_Register  while: error

		AzureIoTProvisioningClient_GetDeviceAndHub

		AzureIoTProvisioningClient_Deinit

		TLS_Socket_Disconnect

	while true

		if connected to internet

				TLS_Socket_Connect

			AzureIoTHubClient_OptionsInit

			AzureIoTHubClient_Init

			AzureIoTHubClient_SetSymetricKey

			AzureIoTHubClient_Connect

			AzureIoTHubClient_SubscribeCloudToDeviceMessage

			AzureIoTHubClient_SubscribeCommand

			AzureIoTHubClient_SubscribeProperties

			AzureIoTHubClient_RequestPropertiesAsync

			AzureIoTMessage_PropertiesInit

			AzureIoTMessage_PropertiesAppend

			AzureIoTMessage_PropertiesAppend

			AzureIoTMessage_PropertiesAppend

			While there are still messages to publish

				AzureIoTHubClient_SentTelemetry

				AZureIoTHubClient_ProcessLoop

				if number of publish is pair
					AzureIoTHubClient_SendPropertiesReported

				endif

				wait a moment

			endwile

			if we are always connected to internet

				AzureIoTHubClient_UnsubscribeProperties

				AzureIoTHubClient_UnsubscribeCommand

				AzureIoTHubClient_UnsubscribeCloudToDeviceMessage

				AzureIoTHubClient_Disconnect

			endif

			TLS_Socket_Disconnect

		endif

		wait a moment

	endwhile








# Détails Fonctions

* Provisioning ou allocution automatique de ressources: Processus permettant d'adapter un service aux besoins d'un client et de le configurer. (Wikipédia)

* Internet of Things: Connecter un appareil à internet afin d'envoyer des données de télémétrie (exemple: température, pourcentage de CPU utilisé etc...)

* MQTT: protocole assurant la communication entre 2 appareils utilisant des technologies différentes. Ce protocole est utilisé à cause de sa légèreté

* `AzureIoT_Init`

	Initialisation de Azure IoT middleware

* `AzureIoTProvisioningClient_Init`

	Initialisation du client pour le provisioning de Azure IoT

* `AzureIoTProvisioningClient_SetSymetricKey`

	Définition de la clé symétrique à utiliser pour l'authentification

* `AzureIoTProvisioningClient_Register`

	- Commence le processus de provisioning... L'appel initial de cette fonction permet de demander au service de faire du provisioning pour le device.
	- Si on appelle à nouveau cette fonction sur le même `AzureIoTProvisioningClient_t`, on aura l'erreur "eAzureIoTErrorPending" qui sera
	renvoyé, et la fonction va simplement regarder si l'autentification a été un succès ou pas.
	- Après un succès, on peut appeler la fonction `AzureIoTProvisioningClient_GetDeviceAndHub` pour obtenir le hub IoT et l'ID du device

* `AzureIoTProvisioningClient_GetDeviceAndHub`

	Après que l'authentification a été réussie, cette fonction permet d'obtenir le hostname du hub IoT et l'ID du device (l'identifiant)

* `AzureIoTProvisioningClient_Deinit`

	Déinitialize le client de provisioning de Azure IoT

* `AzureIoTHubClient_Init`

	Initialise le client Azure IoT Hub

* `AzureIoTHubClient_SetSymetricKey`

	Définit la clé symétrique à utiliser pour l'authentification

* `AzureIoTHubClient_Connect`

	Connection au hub IoT via MQTT (Protocole permettant le transfert cross platform)

* `AzureIoTHubClient_SubscribeCloudToDeviceMessage`

	Subscribe to cloud to device message. Cela signifie qu'on indique au serveur que celui-ci peut envoyer des messages au device... (J'en suis pas sûr...)

* `AzureIoTHubClient_SubscribeCommand`

	Subscribe to commands. Cela signifie qu'on indique au serveur qu'il peut donner au device des commandes que celui-ci exécutera

* `AzureIoTHubClient_SubscribeProperties`

	Subscribe to device properties. Cela signifie que le serveur peut accéder aux propriétés de l'appareil

* `AzureIoTHubClient_RequestPropertiesAsync`

	Requête pour obtenir les propriétés de l'appareil. (Le async indique peut-être que la réponse arrivera d'une manière asynchrone, donc qu'on peut attendre plus ou moins longtemps...)

* `AzureIoTHubClient_SendTelemetry`

	On envoie les données de télémétrie au hub IoT

* `AzureIoTHubClient_ProcessLoop`

	On reçoit touts les messages MQTT entrants, et on s'occupe de gérer la connection MQTT au hub IoT

* `AzureIoTHubClient_SendPropertiesReported`

	Permet d'envoyer les propriétés des appareils signalés au hub Azure IoT (Pour pouvoir utiliser cette fonction, il faut que `AzureIoTHubClient_SubscribeProperties()` soit appelée...)

* `AzureIoTHubClient_UnsubscribeProperties`

	Permet d'indiquer au serveur qu'il ne pourra plus obtenir les propriétés du device

* `AzureIoTHubClient_UnsubscribeCommand`

	Permet d'indiquer au serveur qu'il ne pourra plus envoyer de commandes au device

* `AzureIoTHubClient_UnsubscribeCloudToDeviceMessage`

	Permet d'indiquer au serveur qu'il ne peut plus envoyer de messages au device

* `AzureIoTHubClient_Disconnect`

	Permet de déconnecter le client du hub IoT

* `AzureIoTMessage_PropertiesInit`

	Permet d'initialiser les headers du message

* `AzureIoTMessage_PropertiesAppend`

	Permet d'ajouter des informations au header

* `TLS_Socket_Connect`



* `TLS_Socket_Disconnect`


