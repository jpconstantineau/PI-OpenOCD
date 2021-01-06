# PI-OpenOCD
Automation to configure a Raspberry Pi Zero as an OpenOCD Programmer

## Caveats

The playbook is meant to run on a brand new installation of Raspbian that has not had any configuration changes via `raspi-config` or any other tools, though it _should_ work correctly with an existing installation.

Note: **This modifies your boot config**, and as such you _should not run this playbook on a microSD or other boot volume you're not ready to reformat and re-flash!_

## Setting up the Hardware

It doesn't matter if you set up the software or hardware first, you just need to do both to have a functional programmer

## Setting up the Software

There are two ways you can run this automated setup. You can either run everything on the Raspberry Pi itself (e.g. if you plug in a keyboard, mouse, and monitor), or you can run it from another computer.

### Setup on the Raspberry Pi


  1. Flash the latest Raspberry Pi OS to a microSD card.
  1. Once Raspbian is loaded on the card, insert the card in your Pi, and plug in your Pi to boot it up.
  1. Follow the setup wizard, and if you want to easily be able to log into the Pi later, connect to a WiFi network, and also run the `raspi-config` tool and enable SSH.
  1. The Raspberry Pi should ask to be restarted. Go ahead and restart now, and wait for it to boot back up.
  1. Open the Terminal application (in the launcher or in Menu > Accessories > Terminal).
  1. Install Ansible and Git: `sudo apt update && sudo apt install -y python3-dev python3-pip libyaml-dev libffi-dev git && sudo pip3 install --no-binary pyyaml ansible` (**Warning**: this can take a while, especially on slower Pis!).
  1. Clone this repository to your Pi: `git clone https://github.com/jpconstantineau/PI-OpenOCD.git`
  1. Go into the repository directory: `cd PI-OpenOCD`
  1. Use the local inventory file: `cp inventory-local.example inventory`
  1. Run the Ansible playbook: `ansible-playbook main.yml`
  1. You can shutdown the Pi at this point.

### Setup from another Computer

  1. Make sure you have [Ansible installed](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on your computer.
  1. Flash the latest Raspberry Pi OS a microSD card. Make sure you added an `ssh` file to the boot volume so SSH is enabled on first boot.
  1. Once Raspbian is loaded on the card and you have the `ssh` file in the boot volume, insert the card in your Pi, and plug in your Pi to boot it up.
  1. Make sure you can log into the Pi via SSH. Ideally, add your SSH key to the Pi using `ssh-copy-id`. If you don't, make sure to add the `-k` parameter to the `ansible-playbook` command later so you can enter the SSH password.
  1. Clone or download this repository to your computer.
  1. Use the ssh inventory file: `cp inventory-ssh.example inventory`
  1. Update the IP address in `inventory` to match the IP address or hostname of your Raspberry Pi.
  1. Edit the `config.yml` file to your liking (the defaults should be fine though).
  1. Run the Ansible playbook:

     ```
     ansible-playbook main.yml
     ```

  1. You can shutdown the Pi at this point (log in via SSH then `sudo shutdown now`).

### Powering down the Pi

If you want, and you still have WiFi enabled, or the Pi is otherwise connected to your network, you can log into it via SSH and run `sudo shutdown now` to power it off cleanly.

But I like living on the edge... it's not like the thing is writing a ton of data to the microSD card. When I'm finished using it as a webcam, I just unplug it. Simple as that.

If, for some strange reason, it did end up getting corrupted, I could just re-run this automation to set it up again. No big deal! I haven't had that happen yet, though.

## Known issues

## License

MIT.

This project was inspired from [Jeff Geerling's Pi Webcam Project](https://github.com/geerlingguy/pi-webcam).

## Author

Pierre Constantineau