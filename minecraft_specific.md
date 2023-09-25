# on ubuntu linux:
- wget [ server.jar file copied link address from https://www.minecraft.net/]
- sudo apt update
- sudo apt install openjdk-17-jre-headless 
- sudo java -Xmx1024M -Xms1024M -jar server.jar nogui
    - vanilla
- sudo apt-get install screen
- screen

### forge:
- java -jar [forge installer jar name] --installServer
- run.sh:
```
#!/usr/bin/env sh
# Forge requires a configured set of both JVM and program arguments.
# Add custom JVM arguments to the user_jvm_args.txt
# Add custom program arguments {such as nogui} to this file in the next line before the "$@" or
#  pass them to this script directly
java @user_jvm_args.txt @libraries/net/minecraftforge/forge/1.20.1-47.2.0/unix_args.txt --nogui "$@"
```
- user_jvm_args.txt:
```
# Xmx and Xms set the maximum and minimum RAM usage, respectively.
# They can take any number, followed by an M or a G.
# M means Megabyte, G means Gigabyte.
# For example, to set the maximum to 3GB: -Xmx3G
# To set the minimum to 2.5GB: -Xms2500M

# A good default for a modded server is 4GB.
# Uncomment the next line to set it.
-Xmx8G
```
- chmod +x ./run.sh
- ./run.sh
- will still need to manually make a new empty mods folder

### mc server terminal commands:
- op paulszoo

### downloading worlds:
- sudo apt-get install unzip
- unzip file.zip -d destination_folder

