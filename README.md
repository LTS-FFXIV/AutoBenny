# AutoBenny Plugin
Overview
AutoBenny is a plugin for Final Fantasy XIV (FFXIV) that utilizes the Dalamud framework. Its primary purpose is to automatically cast the spell Benediction on any ally when their health drops below a user-defined health percentage threshold.

Features
Automatically casts Benediction on allies below the specified health percentage threshold.
User can set a custom health percentage threshold with the /bennyhealth command.
Plugin can be enabled or disabled with the /benny command.
Accounts for Benediction's cooldown, character's mana, range limitations, and line of sight.
# Installation
Download the AutoBenny.zip file.
Extract the contents of the zip file into the plugins folder in your Dalamud plugins directory (e.g., %AppData%\XIVLauncher\installedPlugins).
Restart your FFXIV game if it is currently running.
The plugin should now be available in the Dalamud plugin installer. Enable AutoBenny from the plugin list.
# Usage
To enable or disable the plugin, type /benny in the chatbox.
To set the health percentage threshold, type /bennyhealth <value> in the chatbox, where <value> is a number between 0 and 100 representing the health percentage.
# Configuration
The plugin stores its configuration in a PluginConfiguration class. The configuration includes the following property:

HealthThreshold: The percentage (as a decimal) of health remaining below which Benediction will be cast on an ally. Default is set to 0.2 (20%).
Support
For support or to report any issues with the AutoBenny plugin, please open an issue on the plugin's GitHub repository.

# License
AutoBenny is released under the MIT License.
