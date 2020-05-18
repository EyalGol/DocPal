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