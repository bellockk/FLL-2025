# [Setting Up a Development Laptop for FLL](../README.md)

1. Install Windows.

    * Use the pink USB Stick.
    * When asked which partition to install on, delete all partitions, then create a new partition with half the size of the available disk, and select the new partition to intall Windows on.
    * On the "Let's connect you to a network" step, choose "I don't have internet" in the bottom left corner.  We will connect after installation.  If you connect now it will force you to provide a Microsoft ID to finish installation of Windows.
    * Use "FLL" for the username, "maker" for the password, and pick any 3 security questions and answer them all with "maker".

1. Update Windows.

    * Type `win`+`R`, type `control update` and press enter.
    * Click the "Check for updates" button.
    * If requested, reboot the machine to finish installation of updates.
    * Repeat as necessary to install all updates.

1. Auto-login as FLL user.

    * Type `win`+`R`, type `regedit` and press enter.
    * Elevate to admin.
    * Navigate to `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device`
    * In the right pane, double click `DevicePasswordLessBuildVersion` and change the value to `0`.
    * Close the registry editor.
    * Type `win`+`R`, type `netplwiz` and un-check the box "Users must enter a username and password to use this computer."
    * Click "OK"
    * Enter the password "maker" twice and press "OK".
    * Type `win`+`I`, select "Accounts" and then select "Sign-in options".
    * Under "Require sign-in" select "Never" from the drop down box under "If you've been away, when should WIndows require you to sign in again?"
    * Close the Settings dialog.

1. Configure Recyle Bin

    * Right click on the Recycle Bin on the desktop and select "Properties".
    * Click "Don't move files to the Recycle Bin.  Remove files immediately when deleted."

1. Download/Install Git

    <https://git-scm.com/download/win>

1. Install Conda

    Open a `cmd.exe` terminal and execute the following.

    ```bash
    setx HOME "C:\Users\%USERNAME%"
    exit
    ```

    Open a new `cmd.exe` window and execute.

    ```bash
    cd %HOME%
    curl -k -o miniconda.exe https://repo.anaconda.com/miniconda/Miniconda3-py310_23.1.0-1-Windows-x86_64.exe
    start "" /wait miniconda.exe /InstallationType=JustMe /RegisterPython=0 /S /D=%HOME%\.conda
    del /f miniconda.exe
    .conda\condabin\conda activate
    conda create -y -n fll python=3.11 pycodestyle pydocstyle flake8 mypy ipykernel
    exit
    ```

    Open a git bash window, and execute the following.

    ```bash
    ~/.conda/condabin/conda.bat init bash
    printf "conda activate fll\n" >> ~/.bash_profile
    ```

    Close and re-open git bash, and execute the following.

    ```bash
    pip install pybricks
    ```

1. Download/Install VSCode

    <https://code.visualstidio.com/Download>

    Select the User Installer for x64.

1. Download/Install Lego Education Spike Application.

    <https://education.lego.com/en-us/downloads/spike-app/software/>

1. Download/Install EV3 Classroom Application

    <https://education.lego.com/en-us/downloads/mindstorms-ev3/software>

    Select Windows 10 from the dropdown and click "Download Installer".

1. Download/Install LDraw.

    <https://www.ldraw.org/help/getting-started.html>

    * Click on "Windows Instructions", then scroll down to the "All-In_One_Installer (AIOI)", download and install.
    * Make sure to install LDraw and the full offline catalog.

1. Set display to never shut off on power, and to do nothing when the lid is closed.
1. Install Fedora

    * Use the blue USB stick, and boot to the Fedora installer.
    * Select "Install to Hard Drive"
    * For the "What languange would you like to use during the installation process?" make sure "English (United States)" is selected and click "Continue".
    * Click on "Installation Destination", and then click "Done".  This will make all the default selections.
    * Click "Begin Installation", when finished, click "Finish Installaion".
    * Click the top right corner of the screen, press the power button, and select "Power Off" then click the "Power Off" button to confirm.
    * Once the computer is off, remove the thumb drive and power it back on.
    * Once booted select "Start Setup" on the dialog box that appears.
    * Connect to Wifi, and click "Next".
    * Unclick all options on the "Privacy" dialog, and click "Next".
    * Click "Next" on the "Third-Party Repositories" dialog.
    * Click "Skip" on the "Connect Your Online Accounts" dialog.
    * In the "About You" dialog, enter "FLL" in the "Full Name" entry field, and "fll" in the "Username" field.  Ensure the case is correct and click "Next".
    * In the "Set a Password" dialog, enter "maker" twice, and click "Next".
    * Click "Start Using Fedora Linux".
    * Click "No Thanks" in the "Welcome to GNOME" dialog.
    * Open a terminal and execute `sudo dnf -y update --refresh` and enter "maker" when it prompts for a password.
    * When the update is complete restart the computer with the following command `sudo shutdown -r now`.
    * Add the `fll` user to the dialout group.  This is needed to make usb connections.  `sudo usermod -aG dialout fll`.

1. Enable auto login for Linux.

    * Click the windows button and type "users" and press enter.
    * Click the "Unlock" button and enter "maker" for the password.
    * Toggle on the "Automatic Login" button.
    * On the left menu, click on "Privacy".  Unclick the "Automatic Screen Lock" toggle button and change the "Blank Screen Delay" to 15 minutes.
    * Close the Settings dialog.

1. Install Google Chrome

    Execute the following commands in a terminal.

    ```bash
    sudo sh -c "dnf config-manager setopt google-chrome.enabled=1 && dnf install -y google-chrome-stable"
    ```

1. Install needed dependencies.

    Execute the following commands to install conda and create an environment for development.

    ```bash
    sudo dnf -y install zsh vim dfu-util
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
    bash miniconda.sh -b -p ~/.conda
    rm -fr miniconda.sh
    source ~/.conda/bin/activate
    conda init bash
    conda config --system --set auto_activate_base false
    conda create -y -n fll
    conda activate fll
    conda env update --file requirements.yaml
    ```

1. Install `udev` Rules.

    On Linux, `udev` rules are required to allow access to the hub through USB.  Execute the following to install the needed rules.

    ```bash
    pybricksdev udev | sudo tee /etc/udev/rules.d/99-pybricksdev.rules
    ```

1. Install VSCode.

    Execute the following command in a terminal.

    ```bash
    sudo sh -c 'rpm --import https://packages.microsoft.com/keys/microsoft.asc && echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo && dnf -y install code'
    ```
