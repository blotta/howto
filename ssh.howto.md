## Generate RSA key locally

```
$ ssh-keygen -t rsa
```

This will generate two keys under `~/.ssh`; one public key named `id_rsa.pub` and one private key named `id_rsa`. To use a different name and/or location, use `-f /location/of/mykey_rsa`. Keep in mind that the default location is `~/.ssh` and default names are `identity`, `id_rsa`, `id_ecdsa`, `id_ed25519` and `id_dsa`. To choose the key when connecting, specify the location with the `-i` option.


## Copy public key to remote machine with:

`$ ssh-copy-id -i ~/.ssh/id_rsa.pub user@server.com`

>Note: Careful not to copy the private key!

## Using SSH Agent

If passphrase was entered during the key creation, the same will need to be entered at every ssh client session, which is just for the duration of the process/connection. SSH Agent runs for the duration of the local login session, stores encrypted keys and communicates with SSH clients to provide authentication. What it means is that the passphrase only needs to be entered once per local login session.

### Start SSH Agent

Check if the process is already running
`$ ps aux | grep ssh-agent`

To start on the background
`$ eval $(ssh-agent)`

List agent identities
`$ ssh-add -L`

Add key to Agent
`$ ssh-add /location/of/my/privateKey`

## Type less

Simplify connecting through ssh by creating the `~/.ssh/config` file with 0600 permission

```
Host "RServer"
        hostname "serverName"
        user "myRemoteUsername"
        port "2222"
```

Now connect
`$ ssh RServer`
