## The Playbooks

There are 3 playbook files:

* `baseline.yml`: This is a simple playbook that will install the requisite `python` used by Ansible
* `update_fabric.yml`: This will update a fabric once installed
* `site.yml`: This will setup the site witht eh fabric fully.  This will call the *baseline* as well as a precaution in case `baseline.yml` not run.

## The inventory

This playbook looks for the inventory file `testing`.  This can be changed in the supplied `ansible.cfg`.

The Playbook looks for a group called `fabric`.  The supplied inventory shows an example host called `example01`, like so:

```yaml
    example01:
      ansible_host: 10.1.1.1
      ansible_user: someuser
```

Of course, in practice, items like `ansible_user` et al may be defined elsewhere, like the admin's `.ssh/config` or some other inventory system.

This playbook stores host-specific secrets in `host_vars/` for each host.  So, the example above, there is a file called `host_vars/example01.plaintext-yml` that defines the **Wallet** used by the blockchain.  This file can be used as is if renamed to `example01.yml` (provided the host is names this...).  It is not recommended that this be used as is for the following reasons:

* This is plaintext, and the contents are secrets.  It should be encrypted
* This is defined in a public repo.  Ther credentials and mnemonic should be considered unsafe.

To do this properly, use the following...

### Creating the host variables for the Wallet details

#### To create...

In the root of the playbook, create **host specific** variables that house the secrets.  To keep these safe, use `ansible-vault`.

```bash
ansible-wallet create host_vars/example01.yml
```

#### To edit...

Use:

```bash
ansible-wallet edit host_vars/example01.yml
```

In both examples, `example01.yml` is the name of the enrty in the `ansible` inventory.


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