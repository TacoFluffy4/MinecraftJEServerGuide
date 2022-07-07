# Setting up a Minecraft JE server on Windows
This guide exists because I get asked a lot how to setup a Minecraft Java Edition server and have been doing it for almost 10 years. I have had a lot of issues along the way and is a collection of my knowledge and experience. If you have an issue or I missed something, feel free to open an issue on Github but I may not respond, if you solve the problem, say what you did to fix it and close your issue so this guide can be improved. 

## Checking your Java version
1. Open the Windows command prompt.
1. Enter the command `java --version`
If the command is not recognized, the version output does not include "64-Bit", or the argument is not recognized.
(re)install java from the link below.
https://www.oracle.com/java/technologies/downloads/#jdk18-windows
Note: I have had issues with other Java runtimes in the past, so I recommend using Oracles.

## Setting up the server locally
1. Enable show file extensions on Windows
	1. Search Windows for "show file extensions".
	1. Click on show settings for the file extensions options.
	1. Uncheck "Hide extensions for known file types"
	1. Click apply.
1. Go to https://www.minecraft.net/en-us/download/server
1. Download the server jar from the link on the page.
1. Once that file is downloaded, put it in it's own folder.
1. Rename the file to match the command on the website.
	1. Rename the file to match the part of the command that lists the jar file.
	For example: If the command is `java -Xmx1024M -Xms1024M -jar minecraft_server.1.19.jar nogui` then you would want to rename the file to `minecraft_server.1.19.jar` 
1. Go back to the website and copy the command.
1. Create the launch file.
	1. Create a new text file in the folder with the .jar file.
	1. Rename the file to something like `Launch.bat` (The name doesn't matter but it has to end in .bat).
	1. Edit the .bat file and paste the command in.
	Tip: Adding a new line that just has `pause` on it can help you validate if the command runs successfully. This line can be removed later if everything works.
1. Double click the .bat file.
1. Once the program is done running, it should have created a few other files in your folder. Open "eula.txt" and change the line that says `eula=false` to `eula=true`.
1. Double click the .bat file again, and your server will start generating the world.
1. Wait for the server to say `Done` and launch Minecraft.
1. In Minecraft either direct connect, or add a server with the IP set to `localhost` and try to join it.
1. If you are able to connect, you have successfully created a dedicated LAN server and can move on to the next section.

## Port forwarding
This section is the hardest one and most likely to give you issues because it is different depending on what router you have. This section must be done if you want people not on the same network as you to connect.

### Risks with opening ports
Before you open your ports, it is important to know that this can be dangerous to do. Opening ports on their own is not an issue, but if any security exploits are found in the game (or any other program using that port), it could give an attacker access to your computer from outside your network.

### Single router setup
Single router setups are the most common type and if you do not know what kind of setup you have, check how many routers you have, but this is the most likely option.
1. Open up the Windows command prompt again and this time type in `ipconfig`
1. Find which internet adapter you are using, usually either Ethernet or Wireless LAN.
1. Under that adapter Take note of both the IPv4 address and the default gateway.
Tip: If you do not have either of these things, make sure you are looking under the correct adapter.
1. Open your web browser and type in as the URL the default gateway IP from your command prompt.
1. Here you will need to login to your router's gateway. If you do not know the login, you can search the internet for the default login for your router or ask whoever setup your internet.
1. Once in the page, you need to look for the Port Forwarding section.
Tip: This section is sometimes under the advanced category.
1. You are going to want to add a port forward with the settings as follows
The IP to forward to should be the IPv4 address you noted earlier.
The port you want to forward should be 25565 (see later section if you would like to use a different port number).
1. The mode should be set to TCP/UDP, if your router only lets you select one, you might have to create two separate entries, one for UCP and one for TCP.
1. Apply your settings.
	Tip: You may need to restart your router for settings to apply.
1. Restart your server if you still have it open. This is done by typing `stop` in the console and rerunning the .bat file.

### Multirouter setups
If your internet is setup such that you have a router plugged into another router, you may need to port forward slightly differently. For each router that your internet goes through, you will want to forward the ports from the outermost router to the innermost router to your computer. To do this, follow the steps above except use the routers internal IPs instead of your computers, and forward the ports between them to create a path to your computer.

## Testing the server
Now that you have port forwarded, there are only a few steps left.
1. Get your external IP by going to a search engine and typing in "what's my IP".
It should be in the format of 123.123.123.123
1. Open up Minecraft and either direct connect or add a server with that IP.
1. If you cannot connect, you may need to disable the Windows firewall.
TODO: Explain how to disable it.
1. If the firewall is disabled and you still cannot connect, that doesn't mean you did something wrong. Some routers do not let you connect to an internal computer with an external address.
1. Give a friend your IP address and see if they are able to join.
Note: Do not give your IP address to anyone you do not trust, IP addresses should be treated like your real life address.
If your friend can join, that means your port forwarding has worked, and you can move on to the next section.
1. If your friend cannot join but you can (through localhost) that means something likely didn't work in the port forwarding section.
	Tips: double check the IP address, double check the port forward, double check that the firewall is disabled.

## Server configuration
Now that everything is working, you may want to change some settings on the server to make it easier to use.
### Allocating more ram
If the players on your server are experiencing server (block) lag, a common issue is that the server does not have enough ram as the default is only 1gb.
1. Edit the .bat file by right clicking on it and clicking edit.
1. Change the numbers in the -Xmx and -Xms arguments (These numbers define the starting and max number of memory respectively). In general, they should be the same number in Minecraft as the game will use all of it anyways.
1. Each 1024M of memory is equal to 1G (1gb), so allocate depending on your computer specs. I find that 4096M (4gb) is enough for a few players (as of 1.19 at least).

### Setting a whitelist
Enabling the whitelist makes it so only specific players can join, this helps to prevent greifers in the event your IP gets leaked.
1. To enable this, open the sever.properties file in a text editor.
1. Find the line that contains `enforce-whitelist=true|false` and set this to true if it is not already.
1. To add a player to the whitelist you will want to once again restart your server and type the command `whitelist add inGameUserName` where InGameUserName is the username of the player you are allowing.

### Updating the server.
When the game updates, your server will not update automatically, you will need to manually update it.
1. Go back to the server download URL https://www.minecraft.net/en-us/download/server
1. Down the .jar file again
1. Rename the .jar file to the one listed in the command on the website. (See section Setting up the server locally for more information)
1. Update your .bat file to reflect the new file name (Make sure not to lose your memory settings if you changed those).
1. Restart your server.

### TODO: Other port numbers.

### TODO: Using IPv6 addresses.

### TODO: Using a domain as an IP.
