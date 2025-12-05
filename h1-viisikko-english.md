# h1 - The Quintet (Viisikko)

- **Course:** Server Management (Karvinen 2025a)
- **Instructor:** Tero Karvinen
- **Report Author:** Jani Karjalainen

In this homework assignment, I will briefly summarize a few articles using bullet points. In the next assignment, I will install Linux and Salt (salt-minion) inside it.
I will also go through Salt's most important state functions with examples, and I will discuss the term "**idempotent**."

## x) Read and Summarize

* **Salt's most important state functions are: pkg, file, service, user, and cmd (Karvinen 2023).**
    These were new things to me, but truly useful management tools.

* **By adding a project's key to your system, you grant the project full user permissions on your machine (Karvinen 2025b).**
    This is a good reason not to download everything you find online.

* **You can manage thousands of computers using Salt (Karvinen 2018).**
    I wondered, being interested in cybersecurity, is this used much in cyberattacks? Probably.

* **When installing the onedir version of Salt, Salt also installs its own local version of Python, as well as other necessary dependencies for functionality (VMWare Inc. n.d.).**

* **The minion (slave computer) must know where the master is. Minions must also have unique IDs (Karvinen 2018).**
    What happens if the minion computers somehow end up with the same IDs?

* **The report must be precise, repeatable, and easy to read (Karvinen 2006).**
    In my opinion, readability is the most important part of a report.

## b) Installing Salt on Linux (salt-minion)

I installed Salt by utilizing the instructions on the Salt Project's website (VMWare Inc).

I started by creating a directory for public keys with the command:

    $ mkdir -p /etc/apt/keyrings

After this, I downloaded the Salt Project's public key and the Salt software source configuration file with these commands:

```
# 1. Public Key:
$ curl -fsSL [https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public](https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public) | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp  
# 2. Software Source Configuration File:
$ curl -fsSL [https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources](https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources) | sudo tee /etc/apt/sources.list.d/salt.sources
```

Next, I ran the command:

    $ sudo apt update

The command found updates from Saltproject, meaning the previous commands worked correctly:

<img width="608" height="181" alt="image" src="https://github.com/user-attachments/assets/b5910187-099f-4321-a714-df9b6a407526" />

Finally, I installed salt-minion using the command:

    $ sudo apt-get install salt-minion

 <img width="483" height="214" alt="image" src="https://github.com/user-attachments/assets/8a9f0ae1-32de-4450-874c-47d09e569209" />

 After installing the program, I decided to verify that Salt was installed correctly. I used the following command for this purpose:

     $ sudo salt-call --version

I received the response: ``` salt-call 3007.8 (Chlorine)```, meaning the installation was successful and functional.

## c) The Five Most Important State Functions

In this task, I experimented with Salt's most important state functions. I used Tero Karvinen's article on Salt commands as a guide (Karvinen 2018).

pkg - Package Management Module

I tested a command intended to check for and, if necessary, install a program. In this case, "tree":

    $ sudo salt-call --local -l info state.single pkg.installed tree

  <img width="597" height="443" alt="image" src="https://github.com/user-attachments/assets/a8c4f504-b6fe-48da-bb83-a444b2de9661" />

- The command successfully installed the program.

- When running the command again, I noticed that the program checked the installation without making changes.
