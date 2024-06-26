# RemoteRover
 RemoteRover: Your plug and pray platform for seamless remote access.

## Usage
* Download a raspberry pi image in .xz format and place it in the same directory as `env` and `remoterover.sh`
* Change settings in `env` file according to your needs:
  * Check if the image file name corresponds to the one you've just downloaded
  * Take note of the user password (changing the default user name is not supported yet)
  * By default the script copies your SSH public key (`~/.ssh/id_rsa.pub`) to `~/.ssh/authorized_keys` to make passwordless login possible.\
  Feel free to change the key, add multiple ones or to disable this functionality
  * `NET*` variables can be used to set a static IP for eth0 or WLAN interface.\
  To set up a WLAN interface, a `raspi-config nonint` command can be used (`RASPICMDS`) - more info can be found in the env file above raspi-config commands settings
  * `PKGS` are packages that will be automatically installed using apt-get. Enable `RUN_UPDATE` and/or `RUN_UPGRADE` to update the repository and/or upgrade your system before installing them.
  * `EXTRACMDS` are non-filtered commands ran in straight in the raspberry shell - you can do anything you want with them, as long as there aren't any issues with command escaping :)
  * Actions to run are defined by `RUN_*` variables on the bottom of the file. Set the variable to 0 or comment out to disable running it.
* If you're happy with the settings provided by `env` file, just run `remoterover.sh` and wait for it to finish. Your image (`.img`) should be ready to write to SD card.

## Using extra default interfaces:
Make sure `RUN_I2C` and/or `RUN_SPI` are enabled if you want those interfaces to be auto-configured.

### I²C (I2C, IIC)
Use `i2cdetect -y 1` to scan for external I2C devices.

`i2cset` and `i2cget` can be used to write/read data to/from I2C devices.
### SPI
You can use `spi-pipe` to test SPI comms.

## Troubleshooting
If for some reason one or more of the commands fail, check if variables you modified are sane first.

Some raspi-config commands might don't work as expected when ran in qemu, such as enabling I2C or SPI.\
If that's the case, you can use EXTRACMDS to apply your changes manually.

Then, depending on where the execution failed, you might encounter a few issues when trying to run the script again:
* Qemu might still be running: Kill qemu-system-aarch64 manually
* One of the partitions might still be mounted: Check if any of the partitions are still mounted on root/ and boot/, if so, unmount them