It seems a lot of people have issues pairing Logitech bluetooth devices in Linux.

If you are unable to add your bluetooth logitech keyboard or mouse, make sure your bluetooth module is properly loaded in the kernel.

```
sudo modprobe btusb
sudo systemctl restart bluetooth
```

In my case, I had issues with a MX Mini keyboard and a MX Anywhere 3 mouse, both in Ubuntu 24.04. I was unable to discover any of them (either ussing the settings in the GUI or from the terminal with bluetoothctl) until I reloaded the kernel module.

After reloading, pairing working inmediately, and with solaar I can manage all the settings.
