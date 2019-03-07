## Ansible Vault

A Talk Explaining Ansible-Vault Features and Usage

@size[0.8em](2019 | BearingPoint Technology GmbH | T/\S | robert.porisenko@bearingpoint.com)

---

## Introduction

* Used to encrypt sensitive data
* AES256 cipher algorithm
* Very simplistic/basic solution

---

## Usage

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

