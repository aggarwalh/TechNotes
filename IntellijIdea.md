# Intellij

## Setup

* Download Intellij
* Set up Code formatter
  * Configure Intellij to your firmwide style.
  * For my personal projects I prefer google style guide. To set it up install `google-java-format` plugin.
* Set up Code template.
  * Most common item here is to configure copyright/license header. 
* Link routine tasks to "save action" in any file
  * Install "Save Actions" plugin.
  * Then go to Plugin settings: Settings => Other Settings => Save Action
    * Check "Activate Save action on save"
    * Link this action to needed formatting actions. "Optimize Imports" and "Reformat File". For legacy code one can choose 
    "Reformat only changed code" over "Reformat File".
