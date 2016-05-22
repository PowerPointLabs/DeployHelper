# DeployHelper

This is the add-in deployment tool for the [PowerPointLabs](https://github.com/PowerPointLabs/PowerPointLabs) project.  
[![Build status](https://img.shields.io/appveyor/ci/kai33/deployhelper/master.svg)](https://ci.appveyor.com/project/kai33/deployhelper)

## What does it do
- Patch the add-in VSTO manifest to support post-install features (e.g. open a tutorial file after install).
- Produce web installer package or standalone installer package. 
- Upload the package to the server with correct permissions.

## How to use

### Setup
1. Get DeployHelper artifacts [here](https://ci.appveyor.com/project/kai33/deployhelper/build/artifacts).
2. Get DeployHelper.conf [here](https://raw.githubusercontent.com/PowerPointLabs/DeployHelper/master/DeployHelper/DeployHelper/DeployHelper.conf) and fill it with the correct settings<sup>*</sup>.  
   - Mage is a component provided by Visual Studio to sign VSTO manifest
   - Key is the deployment certificate (not the dev one, AKA PowerPointLabs.dev.pfx)
   - SFTP address is the server to upload to
   - SFTP username is the username used to login the server
   - Dev path is the installation folder path on the server for dev-release
   - Release path is the installation folder path on the server for public-release
3. Put DeployHelper artifacts and DeployHelper.conf into a folder, AKA the `publish` folder.
4. In the `publish` folder, create a new folder named `online`. Put [Tutorial.pptx](https://github.com/PowerPointLabs/PowerPointLabs/blob/master/doc/Tutorial.pptx?raw=true), [registry.reg](https://drive.google.com/file/d/0B3iNRZXkTzDTTnZ5am9jWFpvQkU/view?usp=sharing), [PowerPointLabsOnline.SED](https://drive.google.com/file/d/0B3iNRZXkTzDTa21ZcjBEZXBkUWs/view?usp=sharing) and [setup.bat](https://drive.google.com/file/d/0B3iNRZXkTzDTNkhZVjVyVTVVSTQ/view?usp=sharing) into it.
5. In the `publish` folder, create a new folder named `offline`. Put [Tutorial.pptx](https://github.com/PowerPointLabs/PowerPointLabs/blob/master/doc/Tutorial.pptx?raw=true), [PowerPointLabsOffline.SED](https://drive.google.com/file/d/0B3iNRZXkTzDTb0dadzVKN09ENzA/view?usp=sharing) and [setup.exe](https://ci.appveyor.com/project/kai33/powerpointlabs-installer/build/artifacts) (rename PowerPointLabsInstallerUi.exe to setup.exe) into it.
6. Update `TargetName` and `SourceFiles0` of PowerPointLabsOnline.SED and PowerPointLabsOffline.SED with the path of `publish` folder.

### Deploy
1. If there is newer version of Tutorial.pptx, update it in the `online` and `offline` folder.
2. Make sure you're in the correct branch:
   - `dev-release` branch is for **dev-release** with **web/online** installer
   - `release-standalone` branch is for **public-release** with **standalone/offline** installer
   - `release-web` branch is for **public-release** with **web/online** installer
3. Make sure the codes are `ALL GREEN` (can build & all test cases passed).
4. Publish the `PowerPointLabs` project:
   - Update the `Version` and `ReleaseDate` in the `Settings`
   - Update the `ReleaseType` (dev|release) and `InstallerType` (online|offline) in the `Settings`
     - dev: for dev-release and dog-fooding
     - release: for public-release
     - online: to produce and upload web installer package
     - offline: to produce and upload standalone installer package
   - **For InstallerType offline**, leave `Installer Folder URL` blank in the project properties.
   - **For InstallerType online**, update `Installer Folder URL` to be the correct URL (dev or release)<sup>*</sup>.
   - Publish the project to the `publish` folder
5. Run DeployHelper.exe in the `publish` folder and follow the instructions.
6. Verify the deployment<sup>*</sup>.
7. Add tags in Git for the released versions.

<sup>*</sup> Details can be found in the internal documents.
