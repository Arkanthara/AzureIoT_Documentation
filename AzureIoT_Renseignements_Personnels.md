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
* `AzureIoTHubClient_SetSymetricKey`
* `AzureIoTHubClient_Connect`
* `AzureIoTHubClient_SubscribeCloudToDeviceMessage`
* `AzureIoTHubClient_SubscribeCommand`
* `AzureIoTHubClient_SubscribeProperties`
* `AzureIoTHubClient_RequestPropertiesAsync`
* `AzureIoTHubClient_SendTelemetry`
* `AzureIoTHubClient_ProcessLoop`
* `AzureIoTHubClient_SendPropertiesReported`
* `AzureIoTHubClient_UnsubscribeProperties`
* `AzureIoTHubClient_UnsubscribeCommand`
* `AzureIoTHubClient_UnsubscribeCloudToDeviceMessage`
* `AzureIoTHubClient_Disconnect`
* `AzureIoTMessage_PropertiesInit`
* `AzureIoTMessage_PropertiesAppend`
* `TLS_Socket_Connect`
* `TLS_Socket_Disconnect`


