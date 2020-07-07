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
## Logger
### Purpose
Used to log the program flow

## Plugin
### Purpose 
A plugin is the building block of the program. 
As the program has to be expendable to make itself useful and survive the test of time each feature will be implemented as a plugin.
The main types of plugin I expect are: 
- Plugins for objects representation i.e:
    - Function documentation
    - Flow documentation
    - etc...
- Plugins for exportation i.e:
    - Export to IDA
    - Export to Ghidra
    - Export to mind map
    - etc...

### Note
Plugins **wont be abstract**, as each plugin will have it's own purpose and the interface will be made according to the purpose, **but they do have guide lines** and other standardized parts

### Guide Lines
- Plugins will be objects 
    - Plugins wont run in separate threads/process 
- Plugins will provide a "RPC" interface for communication
    - For example a function documentation plugin will give a `get_functions` method that will return a list of function objects
- Plugins will have the option to add other plugins to their "known plugins list" for communication
- All other parts of the plugins but the RPC will be private
- The plugins will get a handle to their part in the database on initialization and will access only the given part

## Manager
### Purpose
The manager will use the configuration to find, initialize and setup communication and the database between the plugins.

## Database manager
### Purpose
Abstraction over the database.
Will initialize the database and have an interface to smartly handout a handle to each plugin.

# Requirements
- python
- db access(Yet to be decided, probably mongoDB)

# Configuration
As the programs environment is pretty static, the configuration will be written in `JSON` and wont have a abstract analyzer wrapping it besides the JSON library 

## Parameters
- plugin_folder_path
- database configuration (to be decided)