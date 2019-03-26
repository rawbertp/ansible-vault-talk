

## @img[clean-img bg-black span-10](presentation/assets/img/ansible.png)Ansible Vault

A Talk Explaining Ansible-Vault Features and Usage

@size[0.5em](2019 | BearingPoint Technology GmbH | T/\S | robert.porisenko@bearingpoint.com)

---

## Introduction

* Ansible core feature
* Used to encrypt sensitive data
* AES256 cipher algorithm
* Very simplistic/basic solution

---?image=presentation/assets/img/hawaii.jpg&size=auto 100%

## What is it<br/>good for?

From DevOps to @color[red](DevOops...)

---

## #RTFM 🧐

@fa[external-link] [User Guide](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

@fa[external-link] [CLI Manual](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)

---

## Some key principles

- Do not store _any_ unencrypted passwords/sensitive data _anywhere_
  - passwords, API keys, SSH keys, GDPR-relevant information, etc. pp...
- No exceptions apply!
- Do not use the _same_ password for _all environments_.
- And of course: Do not store the Vault password in unsecure places/environments!

---

## Usage (1)

```bash
ansible-vault [create|decrypt|edit|encrypt|encrypt_string|rekey|view] [options] [vaultfile.yml]

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

## [En|De]crypt files

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

@fa[info](Use `view` whenever possible)
```bash
ansible-vault view file.yml
```

+++

### Edit encrypted file

```bash
ansible-vault edit file.yml
```

@fa[info](Use `edit` whenvever possible as it helps you not to check in unencrypted files!)

---

### Change password

```bash
ansible-vault rekey file.yml
```

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

## [En|De]crypt variables

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
ansible-vault encrypt_string --vault-id @prompt 'foobar' \
  --name 'the_secret'
```

@fa[info](Instead of `@prompt` a file can be provided)
  
+++

### String decryption

```bash
ansible-vault decrypt_string ...
```

@css[fragment](@fa[frown-o] Nope!)

+++

### String decryption (cont'd)

```bash
ansible-vault view file-with-secret.yml
```

@css[fragment](@fa[frown-o] Nope!)

+++

@snap[midpoint]
@size[3.5em](🙄)
@snapend

+++

### String decryption (cont'd)

_It appears Ansible hasn't fully thought this through..._

@fa[external-link] https://github.com/ansible/ansible/issues/26190

Workarounds:

* Use an ad-hoc command, e.g.:   
  ```bash
  ansible localhost -m debug -a 'var=the_secret' \        
    -e "@group_vars/all/all.yml" --ask-vault-pass
  ```
* Use a dummy playbook

+++

### String encryption bottom line

@fa[plus-circle] Use it if you do not have a lot of sensitive data and/or if it doesn't have to be changed/looked up frequently.

@fa[minus-circle] If you often have to de-/encrypt stuff this approach can get quite cumbersome.

---

## Vault IDs

* Introduced with Ansible 2.4
* Allows multiple passwords (however, all of them have to be provided)
* No, you cannot encrypt one file using multiple passwords!
* I won't go into detail here as I haven't found any valid real-life use case yet?!

+++

### Vault IDs: Usage

Encrypt file:

```bash
ansible-vault encrypt --encrypt-vault-id prod vars/vars.yml
```

Encrypt string:

```bash
ansible-vault encrypt_string --vault-id uat@./uat -n \
  foo this-is-the-secret
```

@fa[external-link] [Further reading/examples](https://learn.redhat.com/t5/Automation-Management-Ansible/Vault-IDs-in-Ansible-2-4/td-p/1531).

---

## P@ssw0rd

@quote[Everything's encrypted using strong passwords!]

@quote[How do we work with it now?]

+++

### Interactive password prompt

```bash
ansible-playbook -i inventory/foo play.yml --ask-vault-pass
```

+++

### Password file

```bash
ansible-playbook -i inventory/foo play.yml \
  --vault-password-file ./foo.pass
```

+++

### Environment variable

You can also set an environment variable pointing to the password file:

```bash
export ANSIBLE_VAULT_PASSWORD_FILE=./foo.pass
ansible-playbook -i inventory/foo play.yml --vault-password-file
```

+++

### Environment variable (cont'd)

The referenced file can also be a script. This allows to inject the password as an environment variable, e.g. using the Jenkins credentials binding plugin.

@size[0.5em](`scripts/echo-vault-pass.sh`)
```bash
#!/bin/bash
if [ "$1x" != "x" ]; then
  echo $1
else
  echo $ALF_VAULT_PASS
fi
```

```bash
export ANSIBLE_VAULT_PASSWORD_FILE=./scripts/echo-vault-pass.sh
```

---

### Demo time

@snap[midpoint]
@fa[hourglass]
@snapend

---

### @fa[comments-o] Q&A

@fa[handshake-o] Thank you for listening!

@fa[question] Any questions?

@fa[bullhorn] Feedback/Custom implementations?

@fa[star] AOB