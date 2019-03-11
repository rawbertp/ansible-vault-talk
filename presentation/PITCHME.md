## Ansible Vault

A Talk Explaining Ansible-Vault Features and Usage

@size[0.8em](2019 | BearingPoint Technology GmbH | T/\S | robert.porisenko@bearingpoint.com)

---

### Introduction

* Ansible core feature
* Used to encrypt sensitive data
* AES256 cipher algorithm
* Very simplistic/basic solution

---?image=presentation/assets/img/hawaii.jpg&size=auto 100%

### @color[white](Why use it?)

---

### Documentation

https://docs.ansible.com/ansible/latest/user_guide/vault.html

https://docs.ansible.com/ansible/latest/cli/ansible-vault.html

---

### Some key principles

* Do not store _any_ unencrypted passwords/sensitive data _anywhere_
  * passwords, API keys, SSH keys, GDPR-relevant information, etc. pp...
* No exceptions apply!
* Do not use the _same_ password for _all environments_.
* And of course: Do not store the Vault password in unsecure places/environments!

---

### Usage (1)

```bash
Usage: ansible-vault [create|decrypt|edit|encrypt|encrypt_string|rekey|view] [options] [vaultfile.yml]

encryption/decryption utility for Ansible data files

Options:
  --ask-vault-pass      ask for vault password
  --new-vault-id=NEW_VAULT_ID
                        the new vault identity to use for rekey
  --new-vault-password-file=NEW_VAULT_PASSWORD_FILE
                        new vault password file for rekey
  --vault-id=VAULT_IDS  the vault identity to use
  --vault-password-file=VAULT_PASSWORD_FILES
                        vault password file
```

---

### Usage (2)

`ansible-vault` can be used to encrypt/decrypt

@ul[square-bullets](false)
* whole files or
* individual strings/values
@ulend

---

### Encrypt/decrypt file

```bash
ansible-vault [create|decrypt|edit|encrypt|rekey|view] [options] [vaultfile.yml]
```

+++

#### Encrypt file

```bash
ansible-vault encrypt file.yml
```

+++

#### Decrypt file

```bash
ansible-vault decrypt file.yml
```

To _view_ the file content only:

```bash
ansible-vault view file.yml
```

+++

#### Edit encrypted file

```bash
ansible-vault edit file.yml
```
@ul[list-bullets-circles](false)
* Use `edit` whenvever possible as it helps you not to check in unencrypted files!
@ulend

---

### Working with encrypted files



---

### Encrypt/Decrypt variables

...