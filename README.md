# Consul and Vault

Consul and Vault are started together in two separate, but linked, docker containers.

Vault is configured to use a `consul` [secret backend](https://www.vaultproject.io/docs/secrets/consul/).

---


## Start Consul and Vault

```bash
docker-compose up -d
```


Login to the Vault image:

```bash
docker exec -it container_name sh
```

Check Vault's status:

```bash
$ vault status
Error checking seal status: Error making API request.

URL: GET http://127.0.0.1:8200/v1/sys/seal-status
Code: 400. Errors:

* server is not yet initialized
```

Because Vault is not yet initialized, it is sealed, that's why Consul will show you a sealed critial status:


### Init Vault

```bash
$ vault operator init
Unseal Key 1: ff
Unseal Key 2: ff
Unseal Key 3: ff
Unseal Key 4: ff
Unseal Key 5: ff
Initial Root Token: ffffff

Vault initialized with 5 keys and a key threshold of 3. Please
securely distribute the above keys. When the Vault is re-sealed,
restarted, or stopped, you must provide at least 3 of these keys
to unseal it again.

Vault does not store the master key. Without at least 3 keys,
your Vault will remain permanently sealed.
```

notice Vault says:

> you must provide at least 3 of these keys to unseal it again

hence it needs to be unsealed 3 times with 3 different keys (out of the 5 above)

### Unsealing Vault

```bash
$ vault operator unseal
```

the Vault is now unsealed:

### Auth with Vault

We can use the `Initial Root Token` from above to auth with the Vault:

```bash
$ vault login
Token (will be hidden):
Successfully authenticated! You are now logged in.
token: ffffff
token_duration: 0
token_policies: [root]
```

---

All done: now you have both Consul and Vault running side by side.
