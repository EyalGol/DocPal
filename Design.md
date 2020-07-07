# DocPal - Design
```puml
note "Event listening is realized using the keyboard library\nit's is included in the project for documentation go here: https://github.com/boppreh/keyboard#api" as keyboard_note

interface IPlugin {
    + Plugin(dict<string, Plugin> plugin_dict)
    + name: string
    - plugin_dict: dict<string, Plugin>
}

note "Every plugin class inherites from this interface\nto simplifie the design this wont be noted" as plugin_interface_note

IPlugin . plugin_interface_note

```