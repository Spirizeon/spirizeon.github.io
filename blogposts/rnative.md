# 🧑‍💻 React Native CLI for Linux
**Author:** Yash Mehrotra
> For Debian Based distributions

For the setup of environment of "React Native" on the Debian based distros you have to follow the following steps .

```plaintext
This part of the setup is only for the general installation 
without any errors.
```

* First you need to visit this [VoltaJs](https://volta.sh/) to install node from it (I am suggesting VoltaJs because its a good version control for node versions. )
    
    OR
    
    You can follow these commands to install VoltaJs and then Node
    
    ```bash
    # install Volta
    curl https://get.volta.sh | bash
    
    #To execute the paths in Bashrc
    source ~/.bashrc
    
    # install Node
    volta install node
    
    # Check the Version of node
    node --version
    ```
    
* Then after the installation of node you need to install the sdk man from this website [SDKman](https://sdkman.io/) to install JAVA from it (I am suggesting SDKman because its a good version control for java versions.)
    
    OR
    
    ```bash
    # To install SDKman in your system
    curl -s "https://get.sdkman.io" | bash
    
    # To execute the paths in Bashrc
    source ~/.bashrc
    
    # To check the versions of JAVA 
    sdk list java
    
    # To install the JAVA version 17.0.9(recommended stabel)
    sdk install java 17.0.9-ms
    ```
    
* After the installation of JAVA you need to install Android Studio you can either install it from [Android Studio](https://developer.android.com/studio?gclid=Cj0KCQiAnfmsBhDfARIsAM7MKi3-LhB2iS3VDTX5F--OA_Cwm_azPDHyh-6ISQPjzsDk6UiBV8R7xY0aAlNnEALw_wcB&gclsrc=aw.ds)
    
* After the installation of Android Studio you need to install all the sdk tools as indicated in the following images (<mark>ticked one's only</mark>)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704893844371/fb6e5f82-c274-4f77-bbde-9ef0173b901c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704893859802/221b837c-ceb4-4b55-8ac3-fa5889b9b815.png align="center")

* After the installation of the following tools now you need to add the path of Android Studio and its Sdk files into your <mark>~/.</mark>**<mark>bashrc</mark>** copy the following path into your ~/.bashrc
    
    ```bash
    export ANDROID_HOME=$HOME/Android/Sdk
    export PATH=$PATH:$ANDROID_HOME/emulator
    export PATH=$PATH:$ANDROID_HOME/platform-tools
    ```
    
* After adding the path and full installation of node and java you can now create a **react-native** project by typing the following command .
    
    ```bash
    #Before writting this command you need get into the 
    #directory of your liking and then use the following command
    npx react-native@latest init AwesomeProject
    ```
    
* Now you need to open **Android Studio** with the {Folder you created with the command present above} and select android folder from the folder like its shown in the image below and then click on the <mark>OK</mark> and then let the Android Studio build the Gradel (**<mark>Gradel build will take some time be patient</mark>**).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704894940608/4740a103-9d35-424a-8391-cb40822629df.png align="center")
    
* **<mark>After the Gradel build it will take time to set the Indexing don't close the Android Studio just after Gradel Build or the app may crash if you do so .</mark>**
    

### Your setup for react-native has been done for Debian based Distros

**Now you can simply get into your directory and type**

```bash
# Please use this command only after getting into ypur project directory and it must conatine a file named with package.json
 npm start
```

## Enjoy!!!
