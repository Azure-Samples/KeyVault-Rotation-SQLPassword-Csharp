# KeyVault-Rotation-SQLPassword-CSharp

Functions generate random password, update sql server admin password, and store password in Key Vault as new version of the same secret.

## Features

This project framework provides the following features:

* Rotation function for sql password triggered by Event Grid (AKVSQLRotation)

* Rotation function for sql password triggered by HTTP call(AKVSQLRotationHttp)

* ARM template for function deployment

* ARM template for adding sql password  to existing function


## Getting Started

Functions generate random password, adds password Key Vault as new version of the same secret and updates password in SQL database.

Functions require following information stored in secret as tags:
$secret.Tags["ValidityPeriodDays"] - number of days, it defines expiration date for new secret
$secret.Tags["CredentialId"] - SQL admin login
$secret.Tags["ProviderAddress"] - SQL Server Resource Id

You can create new secret with above tags and SQL Password as value or add those tags to existing secret. For automated rotation secret expiry date will also be required - it triggers 'SecretNearExpiry' event 30 days before expiry.

There are two available functions performing same rotation:

* AKVSQLRotation - event triggered function, performs sql password rotation triggered by Key Vault events. In this setup 'SecretNearExpiry' event is used which is published 30 days before secret expiration
* AKVSQLRotationHttp - on-demand function with KeyVaultName and Secret name as parameters

Functions are using Function App identity to access Key Vault and existing secret "CredentialId" tag with sql admin login and value with sql admin password to access SQL server.

### Installation

There are 3 ARM templates available:

* [Initial Setup](https://github.com/Azure-Samples/KeyVault-Rotation-SQLPassword-Csharp/tree/main/ARM-Templates#inital-setup)- Creates Key Vault and SQL database if needed. Existing Key Vault and SQL database  can be used instead
* [Function Rotation - Complete Setup](https://github.com/Azure-Samples/KeyVault-Rotation-SQLPassword-Csharp/tree/main/ARM-Templates#azure-sql-password-rotation-functions) - It creates and deploys function app and function code, creates necessary permissions, and Key Vault event subscription for Near Expiry Event for individual secret.
* [Adding additional secrets to existing function](https://github.com/Azure-Samples/KeyVault-Rotation-SQLPassword-Csharp/tree/main/ARM-Templates#add-event-subscription-to-existing-functions) - single function can be used for multiple sql database servers. This template creates new event subscription for the secret.

## Demo

You can find all steps in tutorial below:
[Automate the rotation of a secret for resources that use one set of authentication credentials](https://docs.microsoft.com/azure/key-vault/secrets/tutorial-rotation)
