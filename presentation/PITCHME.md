# Ansible Vault

A Talk Explaining Ansible-Vault Features and Usage

@size[0.8em](2019 | BearingPoint Technology GmbH | T/\S | robert.porisenko@bearingpoint.com)

---

## Introduction

* Ansible core feature
* Used to encrypt sensitive data
* AES256 cipher algorithm
* Very simplistic/basic solution

---?image=presentation/assets/img/hawaii.jpg&size=auto 100%

## @color[white](Why use it?)

---

## Documentation

https://docs.ansible.com/ansible/latest/user_guide/vault.html

https://docs.ansible.com/ansible/latest/cli/ansible-vault.html

---

## Some key principles

* Do not store _any_ unencrypted passwords/sensitive data _anywhere_
  * passwords, API keys, SSH keys, GDPR-relevant information, etc. pp...
* No exceptions apply!
* Do not use the _same_ password for _all environments_.
* And of course: Do not store the Vault password in unsecure places/environments!

---

## Usage (1)

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

## Usage (2)

`ansible-vault` can be used to encrypt/decrypt

@ul[square-bullets](false)
* whole files or
* individual strings/values
@ulend

---

## Encrypt/decrypt file

```bash
ansible-vault [create|decrypt|edit|encrypt|rekey|view] [options] [vaultfile.yml]
```

+++

### Encrypt file

```bash
ansible-vault encrypt file.yml
```

+++

### Decrypt file

```bash
ansible-vault decrypt file.yml
```

To _view_ the file content only:

```bash
ansible-vault view file.yml
```

+++

### Edit encrypted file

```bash
ansible-vault edit file.yml
```
@ul[list-bullets-circles](false)
* Use `edit` whenvever possible as it helps you not to check in unencrypted files!
@ulend

---

## Working with encrypted files

@snap[south-east]
@quote[Working with encrypted files is a PITA!]
@snapend

+++

### Group by stage

Create isolated stage config (group_vars, host_vars) and place the vault-files accordingly.

```
.
├── group_vars
│   ├── all
│   ├── prod
│   └── uat
```

+++

### Use references

Using _references_ (following a _naming convention_) pointing to the actual encrypted value keeps the property files read- and searchable.

@size[0.5em](`group_vars/prod/vars.yml`)
```yaml
a_secret_var: "{{ vault_a_secret_var }}"
```

@size[0.5em](`group_vars/prod/vault.yml`)
```yaml
vault_a_secret_var: p@ssw0rd!
```

---

## Encrypt/Decrypt variables

Instead of whole files you can also encrypt single string values.

```yaml
the_answer: 42
the_secret: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          65373865336330643430383932346333346431386461613831346636633263363265336238626665
          3831316638333634336435396338376638303034373531380a313465663532303131313231613664
          39636239616635386663333166373864396265613231353862623262373437313232346263626661
          6137666331383864640a323438306234623233353837666439666231386265653334333264643330
          6364
```

+++

### String encryption

```bash
ansible-vault encrypt_string --vault-id @prompt 'foobar' --name 'the_secret'
```

* Instead of `@prompt` a file can be provided
  
+++

### String decryption

```bash
ansible-vault decrypt_string ...
```

@css[fragment](Nope!)

+++

### String decryption (cont'd)

```bash
ansible-vault view file-with-secret.yml
```

@css[fragment](Nope!)

+++?image=presentation/assets/img/confused.svg&size=auto 100%

### ???

+++

### String decryption (cont'd)
