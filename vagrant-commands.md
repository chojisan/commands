Typing `vagrant` from the command line will display a list of all available commands.

Be sure that you are in the same directory as the Vagrantfile when running these commands!

## Creating a VM

- `vagrant init`           -- Initialize Vagrant with a Vagrantfile and ./.vagrant directory, using no specified base image. Before you can do vagrant up, you'll need to specify a base image in the Vagrantfile.

- `vagrant init <boxpath>` -- Initialize Vagrant with a specific box. To find a box, go to the [public Vagrant box catalog](https://app.vagrantup.com/boxes/search). When you find one you like, just replace it's name with boxpath. For example, `vagrant init ubuntu/trusty64`.


## Starting a VM
- `vagrant up`                  -- starts vagrant environment (also provisions only on the FIRST vagrant up)

- `vagrant resume`              -- resume a suspended machine (vagrant up works just fine for this as well)

- `vagrant provision`           -- forces reprovisioning of the vagrant machine

- `vagrant reload`              -- restarts vagrant machine, loads new Vagrantfile configuration

- `vagrant reload --provision`  -- restart the virtual machine and force provisioning


## Getting into a VM

- `vagrant ssh`           -- connects to machine via SSH

- `vagrant ssh <boxname>` -- If you give your box a name in your Vagrantfile, you can ssh into it with boxname. Works from any directory.


## Stopping a VM

- `vagrant halt`        -- stops the vagrant machine

- `vagrant suspend`     -- suspends a virtual machine (remembers state)


## Cleaning Up a VM

- `vagrant destroy`     -- stops and deletes all traces of the vagrant machine

- `vagrant destroy -f   -- same as above, without confirmation


## Boxes

- `vagrant box list`              -- see a list of all installed boxes on your computer

- `vagrant box add <name> <url>`  -- download a box image to your computer

- `vagrant box outdated`          -- check for updates vagrant box update

- `vagrant boxes remove <name>`   -- deletes a box from the machine

- `vagrant package`               -- packages a running virtualbox env in a reusable box


## Saving Progress

-`vagrant snapshot save [options] [vm-name] <name>` -- vm-name is often `default`. Allows us to save so that we can rollback at a later time


## Tips

- `vagrant -v`                    -- get the vagrant version

- `vagrant status`                -- outputs status of the vagrant machine

- `vagrant global-status`         -- outputs status of all vagrant machines

- `vagrant global-status --prune` -- same as above, but prunes invalid entries

- `vagrant provision --debug`     -- use the debug flag to increase the verbosity of the output

- `vagrant push`                  -- yes, vagrant can be configured to [deploy code](http://docs.vagrantup.com/v2/push/index.html)!

- `vagrant up --provision | tee provision.log`  -- Runs `vagrant up`, forces provisioning and logs all output to a file


## Plugins

- [vagrant-hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) : `$ vagrant plugin install vagrant-hostsupdater` to update your `/etc/hosts` file automatically each time you start/stop your vagrant box.


## Notes

- If you are using [VVV](https://github.com/varying-vagrant-vagrants/vvv/), you can enable xdebug by running `vagrant ssh` and then `xdebug_on` from the virtual machine's CLI.



### Running Vagrant + VirtualBox from an External Drive

August 7, 2013
I have a MacBook Air with a 128 GB SSD, so I'm always in a bit of a crunch for space on my hard drive. Developing with local VMs provisioned by Vagrant and VirtualBox makes my Drupal (and other) development experience great, but it also quickly fills up the (tiny amount of) remaining space on my SSD!

Here's how you can move your Vagrant files and VirtualBox VMs out of your home folder onto an external hard drive:
1. Copy the .vagrant.d folder from your home folder (~/.vagrant.d) to your external drive (I renamed the folder to vagrant_home: cp -R ~/.vagrant.d /Volumes/[VOLUME_NAME]/vagrant_home"
2. Delete the .vagrant.d folder from your home folder: rm -rf ~/vagrant.d
3. Edit your .bash_profile file, and add the following line (example for Mac OS X): export VAGRANT_HOME="/Volumes/[VOLUME_NAME]/vagrant_home"
4. Open VirtualBox, go to Preferences, and set the Default Machine Folder to a location on your external hard drive (I created a new folder called 'VirtualBox VMs').
At this point, if you download new base boxes, or provision new VMs, they should be on your external drive. (This assumes that you didn't have any existing VMs that you needed to move; in that case, read Moving VirtualBox and Vagrant to an external drive.

Caveats

Unfortunately, there are a couple of downsides to storing VMs externally like this:
* You have to have your hard drive plugged in at all times when running your VMs. (A quick vagrant suspend, at a minimum, is required before unmounting a drive).
* You will need to set the VAGRANT_HOME variable differently for home and work if you use different drives at home and work.
One more note: if you're on a Mac or Linux machine, make sure you use NFS instead of the native folder syncing method. When running a drupal codebase with thousands of files under a shared directory, you'll easily notice at least a 10x or 100x speedup!


When I joined 10gen they gave me a MacBook Pro with an SSD drive. This is wonderful, mainly because it loads StarCraft II really fast. An SSD is like my studio apartment on the Lower East Side: low latency, but a bit cramped. (My apartment is low-latency because it's a 10-minute walk from work. This is not a strained analogy.)
Lately I've needed to spin up a bunch of virtual machines with VirtualBox andVagrant for testing our changes to PyMongo under every conceivable OS, and there's no room for them on my SSD. Even if they run heinously slow on a USB drive, they can't stay in my apartment. Here's how I moved them to an external hard drive:
* Move \~/.vagrant.d to the external drive. I renamed it vagrant_home so I'd be able to see it without ls -a.
* Set VAGRANT_HOME to "/path/to/drive/vagrant_home" in \~/.bash_profile.
* Open the VirtualBox app, open Preferences, and set its Default Machine Folder to "/path/to/drive/VirtualBox VMs".
* Close VirtualBox.
* Move your "VirtualBox VMs" folder to the drive.
* Reopen VirtualBox. You'll see your VMs are listed as "inaccessible". Remove them from the list.
* For each VM in your "VirtualBox VMs" folder on the external drive, browse to its folder in Finder and double-click the .vbox file to restore it to the VirtualBox Manager. (Is there an easier method than this?)
* Finally, move any existing Vagrant directories you've made with vagrant init (these are the directories with a Vagrantfile in each) to the external drive. Since these directories only store metadata you could leave them on your main drive, but it's nice to keep everything together so you could fairly easily plug the whole drive into another machine and start your VMs from there.
Good to go. This has freed up a ton of space on my main drive, and the speed penalty has not been very bad.