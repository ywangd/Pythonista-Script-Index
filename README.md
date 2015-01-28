# Pythonista Command Script Index

The goal of this project is to build a central index to register 
Pythonista command scripts.
It enables easy discovery, installation and management of these scripts in
[StaSh](https://github.com/ywangd/stash). 

The definition of Pythonista Command Script is loose. In theory, any script that
can run in Pythonista qualifies. The word **command** is a vague indication that
these scripts would be more of command-line tools as oppose to GUI programs.

Note: It is currently in test phase and very primitive. Suggestions are welcome.

## Basic structure
Two JSON files are involved for registering a new script.
* **Main Index File**
  ([index.json](https://github.com/ywangd/pythonista-command-script-index/blob/master/index.json))
    - It only has minimal information of each command, including the
      command's name and url pointing to the command's index file.
* **Command Index File**
  ([test.json](https://github.com/ywangd/pythonista-command-script-index/blob/master/test.json)
  for an example)
    - It contains detailed information about a command, e.g. versions, and its
      actual download url.


## Examples
**NOTE**: Comments are for demostration purpose only and are NOT allowed in
actual files.

A sample excerpt of the **Main Index File** could be as follows:
```javascript
{
    "meta_version": "1.0",  // version of this main index file
    "name": "Pythonista Command Script Index",  // name of the main index
    "website": "https://github.com/...",  // url to the main index repo or website

    "commands": {
        "awesome_command_name": {
            "meta_url": "https://github.com/person/repo/info.json", // url to the command's own index file
        },

        "another_awesome_command": {
            "meta_url": "https://github.com/someone/somerepo/info.json#awesome2",
        }
    }
    
}
```

And a sample **Command Index File** is as follows:
```javascript
{
    // Version of this index file
    // It tells SCM how this index file shall be parsed and allows
    // versioning of the index file itself
    "meta_version": "1.0",  
    "name": "awesome_command_name",  // command's name
    "author": "First Last",
    "email": "email@test.com",
    "website": "https://...",  // url to the command's repo or website
    "description": "he is too awesome",

    "releases": [ 
        {
            "version": "1.0", 
            "url": "https://.../awesome.py" // url to download version 1.0 of the command
        },

        {
            "version": "2.0", 
            "url": "https://.../awesome.zip"  // url to download version 2.0 of the command
        },
    ]
}
```

The **Command Index File** allows authors to keep almost all maintenance
information in their own repos and provide its own versionings. Also note many
of these information are optional except only `name` and `url`.

Note the use of `meta_version` for representing the version number of the
index file itself. This allows future changes to the JSON file structure
without breaking old index files, i.e. any future changes will receive a new
version number. The first thing `scm` does is to check the meta_version which
tells `scm` what structure to expect from the index file.

In the **Main Index File**, note the use of `#awesome2` tag for the second
command. This allows multiple commands to share a single **Command Index File**:
```javascript
{
    "awesome1": {
        // ...
    },

    "awesome2": {

        "meta_version": "1.0",  
        "name": "another_awesome_command",  // command's name
        "author": "First Last",
        "email": "email@test.com",
        "website": "https://...",  // url to the command's repo or website
        "description": "...",

        "releases": [ 
            {
                "version": "1.0", 
                // ...
            }
        ]
    }, 

    "awesome3": {
        // ...
    }
}
```

The current supportted file types are:
* Single Python file - it is simply copied to the destination folder
* Single zip file - all contained files are extracted to the destination folder

