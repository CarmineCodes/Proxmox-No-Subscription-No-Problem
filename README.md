# Remove Proxmox Enterprise Repo and Nag Box on logon

This writeup with explain how to remove the enterprise repository that prevents proxmox from being updated and also removing the popup on logon alerting that there is no active subscription. This is not advised to do in a production environment, if you are i advise purchasing a enterprise subscription for support. This guide is intended for homelab use.


## Removing Enterprise Repo For Updating

In your proxmox gui, navigate to your actual proxmox machine in your datacenter, after selecting it, click on updates and then repositories
![image](https://user-images.githubusercontent.com/63487881/203458749-4aab9fff-e5f8-4638-ac74-1d7e342f9f2b.png)


After selecting repositories, you'll see two listings of repos: 
![image](https://user-images.githubusercontent.com/63487881/203458586-48eb767c-ac27-4362-978c-092841238959.png)


Under the second list for the enterprise repo, select the repo 'enterprise.proxmox.com/debian/pve' then select the disbale button
![image](https://user-images.githubusercontent.com/63487881/203459039-f5a1ccfb-29f4-4123-ae42-b8efc684a177.png)

After you disable the enterprise repo, you need to add a new one to replace it

Select the add button, you'll get an alert that you dont have a subscription, click ok and a new box will open
![image](https://user-images.githubusercontent.com/63487881/203459105-d970ef35-0da5-4b1e-ab3d-56ee4a3b53fd.png)

![image](https://user-images.githubusercontent.com/63487881/203459184-d52aab6a-7dbb-461b-a379-74aacc0b6571.png)

In that menu, in the drop down menu select the no-subscription option and then click add
![image](https://user-images.githubusercontent.com/63487881/203459229-88135996-491b-4ccc-97a2-aa10d2becb5c.png)


After you add the new repo, the warnings will go away and instead you'll see

![image](https://user-images.githubusercontent.com/63487881/203459378-d8c1fab2-f979-488f-aa63-8d84b6179dcd.png)

This means you have successfully removed the enterprise repo from being used and can update your proxmox machine

You can now select move back to the updates section and select refresh and update your proxmox machine
![image](https://user-images.githubusercontent.com/63487881/203459610-3aeb41ed-2cb5-490b-9f64-44fafa1be983.png)


After clicking refresh, a box will open and after its finished, it'll show task ok at the end of the text and that means youe proxmox machine is updated.


### Removing Nag Box on Logon

For the current version, this method will work to remove the no valid subscription message that shows everytime you log into your node

Open an ssh connection to your actual proxmox node, not a virtual machine that runs off it but the actual node proxmox is installed on

From there, naavigate to the following directory with the command below
 
 ``` 
  cd /usr/share/javascript/proxmox-widget-toolkit
 ``` 
 
 Then make a backup of the file that is going to be modified with
 
 ```
 cp proxmoxlib.js proxmoxlib.js.bak
 ```
 
 After that, using whatever text editor you prefer (i prefer nano), open the file to edit it. remember you are a root user so any changes you make are certain so be careful before you save the file
 
 ```
 nano proxmoxlib.js
 ```
 
 In nano, use ctrl + w to search for "No Valid Subscription"
 
 the text your looking for will look like this:
 ```
 Ext.Msg.show({
  title: gettext('No valid subscription'),
  ```
 Modify it to look like this:
 
 ```
 void({
  title: gettext('No valid subscription'),
 ```
 
Close and save the file

Restart the proxmox service with:

```
systemctl restart pveproxy.service
```

After the command is finished running, log out and when you log back in the no subscription message should no longer appear
  

