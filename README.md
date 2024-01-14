This is a script created by myself so that if you introduce the IP you want to attack, it check the open ports, and the scan each of them for more info from that port

All suggestions are welcome!!

To run it, 

```
chmod +x autonmap.sh
```

If you want it to use it from any location without the use of the whole path, move it to the /bin location

```
sudo mv autonmap.sh /bin
sudo mv bin/autonmap.sh bin/autonmap
```

Now you can use it in any location by:

```
sudo autonmap <ip>
```
