# Code structure


	Connection to Internet

	AzureIoT_Init

	Set Up Network Credentials				(On initialise une structure contenant des informations d'identification pour le r�seau)

	

			TLS_Socket_Connect

	if DPS (Device Provisioning Service) is enable
	
		AzureIoTProvisioningClient_Init
		
		if symetric key were given in menuconfig

			AzureIoTHubProvisioningClient_SetSymetricKey

		endif
		
		do: AzureIoTProvisioningClient_Register  while: error

		AzureIoTProvisioningClient_GetDeviceAndHub

		AzureIoTProvisioningClient_Deinit

		TLS_Socket_Disconnect

	endif

	while true

		if connected to internet

				TLS_Socket_Connect

			AzureIoTHubClient_OptionsInit

			AzureIoTHubClient_Init

			if symetric key were given in menuconfig

				AzureIoTHubClient_SetSymetricKey

			endif

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








# D�tails Fonctions

* Provisioning ou allocution automatique de ressources: Processus permettant d'adapter un service aux besoins d'un client et de le configurer. (Wikip�dia)

* Internet of Things: Connecter un appareil à internet afin d'envoyer des donn�es de t�l�m�trie (exemple: temp�rature, pourcentage de CPU utilis� etc...)

* MQTT: protocole assurant la communication entre 2 appareils utilisant des technologies diff�rentes. Ce protocole est utilis� à cause de sa l�gèret�

* QOS: Utilisation de mécanismes ou de technologies fonctionnant sur un réseau pour contrôler le trafic et assurer la performance des applications critiques avec une capacité réseau limitée (fortinet.com)

* TLS: s�curisation des �changes faits sur les r�seaux informatiques

* `AzureIoT_Init`

	Initialisation de Azure IoT middleware

* `AzureIoTProvisioningClient_Init`

	Initialisation du client pour le provisioning de Azure IoT

* `AzureIoTProvisioningClient_SetSymetricKey`

	D�finition de la cl� sym�trique � utiliser pour l'authentification

* `AzureIoTProvisioningClient_Register`

	- Commence le processus de provisioning... L'appel initial de cette fonction permet de demander au service de faire du provisioning pour le device.
	- Si on appelle � nouveau cette fonction sur le m�me `AzureIoTProvisioningClient_t`, on aura l'erreur "eAzureIoTErrorPending" qui sera
	renvoy�, et la fonction va simplement regarder si l'autentification a �t� un succ�s ou pas.
	- Apr�s un succ�s, on peut appeler la fonction `AzureIoTProvisioningClient_GetDeviceAndHub` pour obtenir le hub IoT et l'ID du device

* `AzureIoTProvisioningClient_GetDeviceAndHub`

	Apr�s que l'authentification a �t� r�ussie, cette fonction permet d'obtenir le hostname du hub IoT et l'ID du device (l'identifiant)

* `AzureIoTProvisioningClient_Deinit`

	D�initialize le client de provisioning de Azure IoT

* `AzureIoTHubClient_Init`

	Initialise le client Azure IoT Hub

* `AzureIoTHubClient_SetSymetricKey`

	D�finit la cl� sym�trique � utiliser pour l'authentification

* `AzureIoTHubClient_Connect`

	Connection au hub IoT via MQTT (Protocole permettant le transfert cross platform)

* `AzureIoTHubClient_SubscribeCloudToDeviceMessage`

	Subscribe to cloud to device message. Cela signifie qu'on indique au serveur que celui-ci peut envoyer des messages au device... (J'en suis pas s�r...)

* `AzureIoTHubClient_SubscribeCommand`

	Subscribe to commands. Cela signifie qu'on indique au serveur qu'il peut donner au device des commandes que celui-ci ex�cutera

* `AzureIoTHubClient_SubscribeProperties`

	Subscribe to device properties. Cela signifie que le serveur peut acc�der aux propri�t�s de l'appareil

* `AzureIoTHubClient_RequestPropertiesAsync`

	Requ�te pour obtenir les propri�t�s de l'appareil. (Le async indique peut-�tre que la r�ponse arrivera d'une mani�re asynchrone, donc qu'on peut attendre plus ou moins longtemps...)

* `AzureIoTHubClient_SendTelemetry`

	On envoie les donn�es de t�l�m�trie au hub IoT

* `AzureIoTHubClient_ProcessLoop`

	On re�oit touts les messages MQTT entrants, et on s'occupe de g�rer la connection MQTT au hub IoT

* `AzureIoTHubClient_SendPropertiesReported`

	Permet d'envoyer les propri�t�s des appareils signal�s au hub Azure IoT (Pour pouvoir utiliser cette fonction, il faut que `AzureIoTHubClient_SubscribeProperties()` soit appel�e...)

* `AzureIoTHubClient_UnsubscribeProperties`

	Permet d'indiquer au serveur qu'il ne pourra plus obtenir les propri�t�s du device

* `AzureIoTHubClient_UnsubscribeCommand`

	Permet d'indiquer au serveur qu'il ne pourra plus envoyer de commandes au device

* `AzureIoTHubClient_UnsubscribeCloudToDeviceMessage`

	Permet d'indiquer au serveur qu'il ne peut plus envoyer de messages au device

* `AzureIoTHubClient_Disconnect`

	Permet de d�connecter le client du hub IoT

* `AzureIoTMessage_PropertiesInit`

	Permet d'initialiser les headers du message

* `AzureIoTMessage_PropertiesAppend`

	Permet d'ajouter des informations au header

* `TLS_Socket_Connect`



* `TLS_Socket_Disconnect`


# Param�tres des fonctions

* `AzureIoT_Init`

	Aucun

* `AzureIoTProvisioningClient_Init`



* `AzureIoTProvisioningClient_SetSymetricKey`



* `AzureIoTProvisioningClient_Register`


* `AzureIoTProvisioningClient_GetDeviceAndHub`



* `AzureIoTProvisioningClient_Deinit`



* `AzureIoTHubClient_Init`

	- AzureIoTHubClinet_t *
	- hostname
	- hostname length
	- Device ID
	- Device ID length
	- AzureIoTHubClientOptions_t *
	- Buffer (Used for middleware operations and MQTT messages)
	- Buffer length
	- AzureIoTHubGetCurrentTimeFunc_t
	- AzureIoTTransportInterface_t *

* `AzureIoTHubClient_SetSymetricKey`



* `AzureIoTHubClient_Connect`

	- AzureIoTHubClient_t *
	- new session ?
	- Other session already exist ?
	- wait time

* `AzureIoTHubClient_SubscribeCloudToDeviceMessage`

	- AzureIoTHubClient_t
	- Other function to invoke when a cloud message arrive
	- Other context
	- Wait time for say that all is done
	
* `AzureIoTHubClient_SubscribeCommand`

	- AzureIoTHubClient_t *
	- Other function to invoke when a command arrive
	- other context
	- wait time for say all is done

* `AzureIoTHubClient_SubscribeProperties`

	- AzureIoTHubClient_t *
	- Other function to invoke when device properties arrive
	- Other context
	- Wait time for say that all is done

* `AzureIoTHubClient_RequestPropertiesAsync`

	- AzureIoTHubClient_t *

* `AzureIoTHubClient_SendTelemetry`

	- AzureIoTHubClient_t*
	- telemetry data
	- telemetry data length
	- header (properties bag)
	- QOS use for telemetry (I don't know what is it...)
	- telemetry ID (I don't know what is it... Perhaps, it's the id that permit to server to detect that data comes from sensor and not cpu for instance...)

* `AzureIoTHubClient_ProcessLoop`

	- AzureIoTHubClient_t *
	- Timeout

* `AzureIoTHubClient_SendPropertiesReported`

	- AzureIoTHubClient_t *
	- payload of properties formatted
	- payload length
	- request ID used to send reported property

* `AzureIoTHubClient_UnsubscribeProperties`

	- AzureIoTHubClient_t *

* `AzureIoTHubClient_UnsubscribeCommand`

	- AzureIoTHubClient_t *

* `AzureIoTHubClient_UnsubscribeCloudToDeviceMessage`

	- AzureIoTHubClient_t *

* `AzureIoTHubClient_Disconnect`

	- AzureIoTHubClient_t *

* `AzureIoTMessage_PropertiesInit`

	- AzureIoTMessageProperties_t *
	- Buffer
	- length of properties already written
	- Buffer length

* `AzureIoTMessage_PropertiesAppend`

	- AzureIoTMessageProperties_t *
	- property name
	- property length name
	- property value
	- property length value
