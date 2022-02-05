# Using Ansible with Podman

###

Here is my start at using Ansible to work with Podman. I like things the hard way, apparently, and I completely bypassed Docker and Docker-compose on my journey into OCI-compliant containers. Well, not completely. I did get my unifi and plex containers up and running with podman-compose, just to get started. Here is a very brief intro for how to use this. Just enough to get in trouble. The documentation for the ansible modules is quite good.

Read the docs for each container to make sure you're using the correct variables.

## Dependencies

First you need to have ansible installed. I like using pip for that. Make sure you have pip installed with your distro's package manager.

Fedora:
```bash
sudo dnf install python3-pip
```
openSUSE:
```bash
sudo zypper in python39-pip
```

Then use pip to install ansible, I like to install wheel first.
```bash
pip install wheel
pip install ansible
```

If you haven't already, make sure /home/$USER/.local/bin is in your path.
```bash
echo $PATH
export PATH=/home/$USER/.local/bin:$PATH
```

Make it persistent.
openSUSE:
```bash
echo "export PATH=/home/$USER/.local/bin:$PATH" >> /home/$USER/.profile
```
Fedora:
```bash
echo "export PATH=/home/$USER/.local/bin:$PATH" >> /home/$USER/.bashrc
```

## Using this playbook

Clone the playbook and enter the directory. Then run:
```bash
ansible-galaxy install -r requirements.yml
```

Check the gatewaylp.yml file in the host_vars directory, it has some variables you need to define before you can run the playbook. I set them in my vault file outside the project directory.

You can run the whole playbook and it will set up these containers.

I set it up with tags. That way you can select which tasks to run. For example:
```bash
ansible-playbook -K container.yml --tags "swag,pihole,plex"
```

## Container name resolution

I found that at first, container name resolution was working well, until I started using pihole as a container. Then my reverse proxy wouldn't work with name resolution so I had to set up the proxy confs with ip addresses. As long as the "swag-proxy" network is the first user defined network, then the ip addresses for heimdall, pihole, and adguard will work just fine.
