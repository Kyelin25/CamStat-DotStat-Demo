# DotStat Docker demo

Demonstration of using docker-compose to stand up a full DotStat environment. Not to be used in production or as an example of best practice, especially as pertains to security.

## Prerequisites

Must have Docker installed.

## Quickstart

- Navigate to docker folder
- Run `docker-compose -f docker-compose-backend.yml up` to start up the backend or .NET Core services and databases
- Run `docker-compose -f docker-compose.yml up` to start the DLM and DE services and Keycloak
- Navigate to http://localhost:8080 and enter the admin console (or go directly to http://localhost:8080/auth/admin/master/console). The username is `admin` and the password is `password`
- Create a new Realm called `OECD`
- Within that realm, create a new client called `app`
- Enable implicit flow for the client
- Add http://localhost:7000*, http://localhost:7001*, http://localhost:93* and http://localhost:94* to the Valid Redirect Uris for the client
- Add http://localhost:7000, http://localhost:7001, http://localhost:93 and http://localhost:94 to the Web Origins
- Add Protocol Mapper of type Group Membership, Token Claim Name `groups`, with "Full group path" unticked
- Add a group called `admins` to the OECD realm. 
- Add a user called `dotstat_admin` to the OECD realm. Make sure they have a valid email address, tick "Email Verified".
- Reset the user's password to something you'll remember. Untick "Temporary" when doing so.
- Add the `dotstat_admin` user to the `admins` group
- Navigate to http://localhost:7000 for the Data Lifecycle Manager, or http://localhost:7001 for Data Explorer
- The Data Lifecycle Manager will automatically redirect you to Keycloak, where you should log in as the `dotstat_admin` user.
- Data Explorer will not automatically redirect you, but you may click "Log in" to be redirected to Keycloak and log in as the `dotstat_admin` user.

That's it, you're done.

## Emails

Emails sent from Data Explorer (for the share service for example) will actually arrive at the `dotstat_admin` user's email address.

Emails sent by the Transfer service (for example when loading or transferring data using the Data Lifecycle Manager) will be captured by the MailHog SMTP capturing server. Navigate to http://localhost:8025/ to see them. Alternatively
go to the .env file in the docker folder and change the SMTP service config to point to the SMTP server of your choice.
