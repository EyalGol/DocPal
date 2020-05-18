# **Specification document - DocPal**
# General
Efficient documentation of program research and reversing.

## Problem
- Usually it is required to document in multiple places i.e
    - IDA
    - Thought dump
    - Final documentation
    - Mind Map
    - Maybe even Ghidra and more...
- Good practice in documentation is hard and takes a lot of time
    - Proper formatting of function, flows etc.
    - Making tables, and complicated flows is cumbersome and annoying and thus its being avoided in place of quicker and dirtier documentation
- Connection between the mind map and other documentation
- Every program has different shortcuts and ways to document

## Solution
The solution is to essentially build a platform that will connect all of the above in a sensible way that is to store the information in a separate database, this way you can interact with it freely, miss and match, connect different parts, etc.

Just to give a taste of the possibilities, imagine pressing a shortcut that will popup a window that will automatically fetch all the information you already added in you current function in IDA but will give you a couple of additional fields that you'll be able to fill in like tags, connection, flow etc. Or pressing a key to popup a window with a "Assumption" field and a "Result" field that you will just "write tab write enter" and forget about. and all of the above will be stored nicely in a database for later export whenever and however you'd like.
### Initial
Platform to store documentation fluidly in a database, the program will provide a platform to add plugins, the plugins will do things as interact with IDA to fetch information from in or give the user a GUI to interact with etc...

### Optional
Crete a couple of plugins to jumpstart the project i.e
- Plugin that will fetch and store information from IDA and will give a nice interface to interact with the information afterwards
    - Same but for Ghidra
- Plugin to import the information into IDA/Ghidra
- Plugin to add tags to function in IDA/Ghidra (Used for mindmaps)
- Plugin to export the chosen parts of the documentation in a pdf form
- Plugin to export the chosen parts of the documentation to confluence
- Plugin to give the user a GUI, and a shortcut platform for the platform

# Components
## Diagram
```puml
[Logger]
[Plugin Manager] -right- (GUI plugin)
[Plugin Manager] -left- (IDA plugin)
[Plugin Manager] -down- (Ghidra plugin)
[Plugin Manager] -up- (Confluence plugin)
```
## Logger
Used to log the program flow

## Plugin Manager
Will provide a platform for the user to add his own plugins, the plugin manager will bootstrap and initiate plugins, and provide an API to interact with the plugins.

# Interfaces
## Plugin Manager
- get_plugin_list
    - Return a list containing all the initialized plugin object 
- stop
    - Gracefully stop all plugins
- start
    - Start all plugins

### Usecase
- Initialize
    - import and try to initialize added plugins
        - In case of failed import or initialization write to log and continue
        - In case of successful initialization write to log and continue
    - Return list of initialized plugins when asked to

## Plugin 
A plugin will have a standard interface and a customizable interface, in addition the plugin on initialization will get a list of all the other plugins and a "handler" to its own table in the database
### Init required arguments
- database_handler
- plugin_list
### Standard(Required)
- start
- stop
- get_name