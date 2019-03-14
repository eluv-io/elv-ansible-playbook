### The inventory

This playbook looks for the inventory file `testing`.  This can be cahnged in the supplied `ansible.cfg`.

The Playbook looks for a group called `fabric`.  The supplied inventory shows an example host called `example01`, like so:

```ini
[fabric]
example01 ansible_user=admin.user ansible_host=1.2.3.4
```

Of course, in practice, items like `ansible_user` et al may be defined elsewhere.

This playbook stores host-specific secrets in `host_vars/` for each host.  So, the example above, there is a file called `host_vars/example01.yml` that defines the **Wallet** used by the blockchain.

### Creating the host variables for the Wallet details

#### To create...

In the root of the playbook, create **host specific** variables that house the secrets.  To keep these safe, use `ansible-vault`.

```bash
ansible-wallet create host_vars/somehost.yml
```

#### To edit...

Use:

```bash
ansible-wallet edit host_vars/somehost.yml
```

In both examples, `somehost.yml` is the name of the enrty in the `ansible` inventory.


The format of the file ihost variable file is:

```yaml
---
mnemonic12: "<bip39>"
wallet_secret: "<passphrase>"
```

In this example, the **bip39** portion is a mnemonic format called BIP39.  To generate one, use a tool.  Here is an example of one:

```bash
npm i bip39-cli
bip39-cli generate
```

If `bip39-cli` is not in the user `$PATH` then use `npm bin` to find out where the binary is.  An alternative is to use a host BIP39 gerenator.

The **passphrase** is a secret

## Known Issues

[] Current `qfab` installer will fail is installing over a previous installation
