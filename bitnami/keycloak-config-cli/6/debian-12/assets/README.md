# Rebuilding the keycloak-config-cli jar file

- Clone the [repo](https://github.com/adorsys/keycloak-config-cli)

- Follow the [build instructions](https://github.com/adorsys/keycloak-config-cli)

- You should end up with a new `keycloak-config-cli.jar` in the `target/` directory. Copy that file into this `assets` dir and you should be able to `make build` / `make push` a new container image which contains the updated jar file. 