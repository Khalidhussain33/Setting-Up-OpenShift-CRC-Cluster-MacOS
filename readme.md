# 
<h1 align="center">Welcome to Setting Up OpenShift Custer Using Code Ready Containers (CRC) on MacOS 👋</h1>
<p align="center">
  <img src="https://img.shields.io/badge/Red%20Hat%20Open%20Shift-4.2%20or%20newer-EE0000?labelColor=EE0000&color=grey&logo=Red%20Hat%20Open%20Shift&style=social%22" />
  <a href="https://twitter.com/mfaisaltariq01" target="_blank">
    <img alt="Twitter: mfaisaltariq01" src="https://img.shields.io/twitter/follow/mfaisaltariq01.svg?style=social" />
  </a>
</p>

### **Downloading Packaged Containers**
------

Go to the [LINK](https://developers.redhat.com/products/codeready-containers) and Click on **Download CodeReady Containers**. It will route you to the login page. Login using your RedHat Credentials or you can signup for a new account and then login. 

Once LoggedIn click on the **macOS: Download (HyperKit)** link and it will download a tarball. Note that that the cluster will be setup on HyperKit which comes pre installed on macOS previous versions had an option to setup with VirtualBox as well.

A secret is also required to pull containers, the secret can also be downloaded from a link given on the same page. Click on the **Download Pull Secret** link to download the file. 

![](./images/1.png)

### **Setting up CRC and Configurations**
------
Once downloaded extract the tarball 

```
tar -xvf crc-macos-amd64.tar.xz
```

Move the **CRC** binary to a folder where you can directly access the command
```
sudo mv crc-macos-1.9.0-amd64/crc /usr/local/bin/
```

Run the command to verify that that crc binary is accessible globally.
```
crc --help
```
> Sometimes it might popup a message that says that it is from an unidentified developer. Allow execution from **Security and Privacy** section in the settings.

To setup CRC run the following command
```
crc setup
```

If the setup is successful you should see the following output.

![](./images/2.png)


### **Starting the Cluster**
------

Run the `crc start -h` command to check out the different options to specify custom options for increased memory and cpu usage. I'll be using the default minimum requirements to run the cluster. While starting the cluster we'll have to provide the **Secret** that we downloaded previously.

```
crc start -p <path>/pull-secret 
```

Once started, a few instructions on how to setup environment and access the console will be presented on the terminal.
![](./images/3.png)

Run the following commands given in the instructions
```
crc oc-env
eval $(crc oc-env)
```

Now lets login to the cluster using the **developer** account. You can also use the admin account, the password can be found for the admin in the instructions printed on the terminal.

```
oc login -u developer -p developer https://api.crc.testing:6443
```
![](./images/4.png)


After logging into the cluster we can create projects from the CLI or the Web Console. To access the Web Console type in the `crc console` command to open it up in the web browser. Choose the admin account or developer account to login, provide username and password to login.
![](./images/5.png)

## **Creating a test project using a template (S2I)**
------

The test project can be setup by either using the **console** or the **cli**. Lets visit both of these methodologies.

### **Using Console**
------

The first step is to create a project. Make sure that the **Developer** tab is selected and click on **Project --> Create Project** in the main window.
![](./images/6.png)

Name the project **test-project** and fill in the other fields. Click on **Create**.
![](./images/7.png)

Click on the **Catalog** section and then the select the **Apache HTTP Server** from the items section. Click on **Instantiate Template** and note the **GitHub URL**, it will be used when we create the app using the CLI.
![](./images/8.png)

![](./images/9.png)

![](./images/10.png)

![](./images/11.png)

Wait for a few minutes, until the **Status** turns to **Ready**.

![](./images/12.png)

Select the **Topology** tab on the left and click on **Open URL** to visit the deployed Apache Server web page.

![](./images/13.png)


![](./images/14.png)

### **Using CLI**
------

## Resetting the Environment
Delete the application and project to reset the environment by running the following commands on the console.

```
oc delete all -l app=httpd-example
oc delete project test-project
```
![](./images/15.png)

Create the project from the CLI by running the following command.

```
oc new-project test-project
```

![](./images/16.png)

Now the application can be deployed using the **Github URL** that we have copied previously.

Clone the repository and move into the templates folder.

```
git clone https://github.com/sclorg/httpd-ex.git
cd httpd-ex/openshift/templates/
```

Run the create app command by providing the **template.json** as an applicaiton template.

```
oc new-app -f httpd.json
```
![](./images/17.png)

Wait for a few mintues until the deploment and build has successfully completed and the pod has been deployed. Check the status by running the `oc get all` command.

![](./images/18.png)

Copy the application URL from the routes section and open it up in the browser.

## Author

👤 **Muhammad Faisal Tariq**

<p>
  <a href="https://twitter.com/mfaisaltariq01" target="_blank">
    <img alt="Twitter: mfaisaltariq01" src="https://img.shields.io/badge/Twitter%20%20%20-Follow-13324B?logoWidth=24&labelColor=13324B&color=grey&logo=Twitter&style=social%22" />
  </a>
  </br>
  <a href="https://github.com/mfaisaltariq" target="_blank">
    <img alt="Github: mfaisaltariq" src="https://img.shields.io/badge/GitHub-Follow-181717?logoWidth=24&labelColor=181717&color=grey&logo=GitHub&style=social%22" />
  </a>
  </br>
  <a href="https://linkedin.com/in/mfaisaltariq" target="_blank">
    <img alt="LinkedIn: mfaisaltariq" src="https://img.shields.io/badge/LinkedIn-Follow-0077b5?labelColor=0077b5&color=grey&logo=LinkedIn&style=social%22" />
  </a>
  </br>
  <a href="https://www.youtube.com/c/DesktopDev_mfaisaltariq" target="_blank">
    <img alt="YouTube: mfaisaltariq" src="https://img.shields.io/badge/YouTube-Subscribe-FF0000?labelColor=FF0000&color=grey&logo=YouTube&style=social%22" />
  </a>
</p>

## Show your support

Give a ⭐️ if this project helped you!















