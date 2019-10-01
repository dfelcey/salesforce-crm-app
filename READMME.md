# Overview
This Salesforce CRM application provides operations for fetching changes to Contacts. Either a single or multiple contacts can be retrieved, using an optional *modified since filter*.

## Individual Contacts
Individual contacts are looked up using the Salesforce Contact Id. If a contact is not found then a 404 response will be returned.

## Multiple Contacts
Multiple Contacts can be retrieved and filtered using  date-time *modified since filter* parameter, in the format `2015-07-04T21:00:00`

This filter can be used to return both newly created and modified contacts since the filter time. Contacts are returned in ascending sorted order, where the oldest modified contact will be the first

# Configuration
Before deploying the application it needs to be configured with the correct Salesforce connection parameters. These are set in the 3 configuration files in the dire src/main/resources These  have the name `<env>-config.properties`, where `<env>` is the environment the application is deployed to (`dev`, `test` or `prod`).

The environment parameter that determines which configuration file is used is set using a system property, `mule.env`, that can be set at runtime. The default environment is `test`