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
### Initial
API that that provides an interface to manage documentation that will be internally stores in a database, and will have the option to write plugins to add new documentation forms i.e flows, function, etc... and to add new export forms i.e export into IDA, export markdown, export PDF, etc...

### Optional
Front end interface that will give an easy way to add documentation i.e quick editable shortcuts to add documentation, script integration (for example if you add a function it will check if IDA runs and if it does it will take the function address and automatically and the documentation into IDA) etc...

### Note
In addition the platform will provide a flexible and modular way to create and add plugins and other features in whatever fashion you like, the "problems and solutions" are only my reasons and thoughts about the case but the platform will be generic.

# Components
## Diagram
```puml
component UserAPI
component DatabaseManager
component Logger
component PluginManager

Logger --* UserAPI
DatabaseManager --* UserAPI
PluginManager --* UserAPI
```
## User API
Provides an API for the user, will interact with all of the other components

## Database Manager
Abstraction of database management

## Logger
Used to log the program flow

## Plugin Manager
Will provide a platform for the user to add his own plugins
- Export plugins
    - Add new ways to export the data and create files from the database
- Database plugins
    - Add new formats and ways to search the database
- API plugins
    - Add new plugin calls and options

 The list above is more of a logical separation that will not necessarily be implemented this way in the final version. 

# Interfaces
## Plugin Manager
The plugin manager will read and initialize all of the plugins on its own initialization

### Interface
- get_list
    - Return a list of all plugins

### Usecase
- Initialize
    - import and try to initialize added plugins
        - In case of failed import or initialization write to log and continue
        - In case of successful initialization write to log and continue
    - Return list of initialized plugins when asked to

## Plugin 
A plugin will contain one or more "sub-plugins" for each of the components i.e in one plugin there will be a sub-plugin for the User API that will add an API call and an Database Manager sub-plugin that will add a new format of adding data to the database, the plugin will also have a unique name
- get_dict
    - Will return a dict of all the sub plugins where the key is the class of the sub plugin and the value is the object itself

## Sub Plugins