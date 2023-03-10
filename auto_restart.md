Yes, it is possible to automatically run the command sudo systemctl enable nginx.service when the server is restarted. You can do this by adding a script to the /etc/rc.local file.

Here are the steps:

1 Open the /etc/rc.local file in a text editor:

```
sudo nano /etc/rc.local
```

2. Add the following line to the end of the file, before the exit 0 line:

```
sudo systemctl enable nginx.service
```

Note: Replace nginx.service with the name of the service you want to enable.

3. Save the file and exit the editor.

4. Make the script executable:

```
sudo chmod +x /etc/rc.local
```

5. Reboot the server to test the auto-enable feature:

```
sudo reboot
```
