# <a href="http://www.freenas.org/download-freenas-release.html" target="_blank">FreeNAS ISO</a>
# <a href="http://freenas.2trux.com/FreeNAS.pdf" target="_blank">FreeNAS PDF</a>

# Commands
Check Disks
```sh
zpool status -v
```
Boot
```sh
If you want to reinstall FreeNAS, I'd install the same version you were running before on a new flash drive.
Look here for older versions: http://download.freenas.org/

If you saved the configuration file, you can restore it using the webGUI after you do the fresh install.
If not, just do an autoimport of your pool and reconfigure the server. 
```
