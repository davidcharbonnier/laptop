# laptop

## Features

This tool setup a CLI development environment focussed on DevOps work. It does the following:
- Install a shell (currently only ZSH is supported) with Starship prompt, a proper font (NerdFont) and some basic tools
- Install a text editor (currently only NeoVim is supported) with a compete IDE setup (LunarVim)
- Install Docker and related tools (like compose, buildx)
- Install Kubernetes related tools (a few Krew plugins as well as static tools like k9s or kind)
- Install Cloud related tools (like gcloud or AWS CLI)
- Install Terraform related tools (linter, security scanner, doc generation)
- Install some common devops related tools (like ArgoCD, Vault, K6)
- Install some common development related tools (like Vagrant and some git related tools)
- Install configuration files (aka dotfiles) from Github using Chezmoi

Whenever possible, we tend to use official Docker images and use an alias to corresponding `docker run` command to avoid installing tools locally.

## Install requirements

You first need to install base system packages:

- on Ubuntu:
  
  ```bash
  sudo apt-get update
  sudo apt-get install python3-virtualenv python3-apt
  ```
- on Fedora
  
  ```bash
  sudo yum update
  sudo yum install python3-virtualenv python3-dnf
  ```
Then, you can install requirements for the project to run:

```bash
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
ansible-galaxy collection install -r requirements.yml
```

## Run playbook

To run the plabook:

```bash
ansible-playbook -i inventory site.yml --extra-vars "ansible_python_interpreter=/usr/bin/python3"
```

## Development

In order to test the tool during development, you need to have Vagrant installed as well as libvirt provider.
Then, you have to run the following:

```bash
vagrant up
```

You can switch from Ubuntu 22.04 to Fedora 39 boxes in the Vagrantfile.

If you need to run the ansible playbook multiple times, after a first `vagrant up`, you can run in another terminal the following:

```bash
vagrant rsync-auto
```

Then, launch provisionning again:

```bash
vagrant provision
# or
vagrant up --provision
```

When you're done, you can tear down your development environment:

```bash
vagrant destroy
```