# Keycloak as Gerrit OAuth2 provider

[Gerrit](https://www.gerritcodereview.com) is a code review and project
management tool for Git based projects.

[Keycloak](https://www.keycloak.org/) is open source Identity and Access
Management tool.

## Objective

This doument provides a step-by-step tutorial how to set-up Keycloak as
OAuth2 provider for Gerrit Code Review for development only. For production
HTTPS protocol must be configured.

## Prerequisites

- Linux or MacOS operating system
- Docker-compose is installed

## Documentation

1. Install Keycloak official Docker image from this repository and start it:

```bash
  $ git clone https://github.com/jboss-dockerfiles/keycloak
  $ cd keycloak/docker-compose-examples
  $ docker-compose -f keycloak-postgres.yml up
```

2. Login to Keycloak using admin/Pa55w0rd credentials and import keycloak
[json file](../master/resources/keycloak-gerrit-client-export.json).

3. Create test user: John Doe, with username "jdoe", email: john@doe.org, with
password "secret", Temporary=OFF.

4. Configure gerrit according to provided [gerrit.config](../master/resources/gerrit.config).
Note this Keycloak OAuth2 provider configuration section:

```
[plugin "gerrit-oauth-provider-keycloak-oauth"]
        root-url = http://localhost:8080
        realm = master
        client-id = gerrit
        client-secret = f9980771-0172-43df-bc5b-98bc2f9477d0
```

5. Set up gerrit site using latest released gerrit.war and select OAUTH
authentication scheme using:

```bash
  $ java -jar gerrit.war init -d gerrit_site_oauth
```

6. Download gerrit-oauth-provider plugin from [here](https://github.com/davido/gerrit-oauth-provider/releases)
and copy it to <gerrit_site>/plugins directory

7. Start gerrit using: `<gerrit_site>/bin/gerrit.sh start`

8. Enter gerrit URL in browser: http://localhost:8081 and hit "Sign In" button

9. Keycloak Login Dialog should appear

10. Enter user: "jdoe" and password: "secret"

11. You are redirected to gerrit and the first user/admin John Doe is created
in gerrit with the right user name and email address.

12. Congrats, you have Gerrit / Keycloak OAuth2 integration up and running.
 