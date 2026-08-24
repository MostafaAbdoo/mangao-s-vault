https://onlineyamltools.com/validate-yaml
https://jsonformatter.org/yaml-viewer
Applications need a standard language so that it's components can talk to each other and YAML is one of them just like JSON, XML -> *data serialization languages*
YAML is used in creating configuration files, log files, inter-process messaging, cross-language data sharing and so on
in YAML spaces are important
```yaml
# Key-Value Pairs
 app_name: "My Smart App"   # space before the value
 version: 1.2 
 is_active: true
 
 # Nested Object (Dictionary) -- Dictionary/Hashes/Mapping are unordered
 user:
  username: "johndoe" 
  email: "john@example.com" 
  settings:
   theme: "dark" 
   notifications: true
# u can represent dictionaries using `{}` as well user: {name: mostafa, age: 20}
```
`---` indicates beginning of a document and `...` indicates the end ->*in case of writing multiple documents in on file*

if u have any special character use quotes to prevent any issue, and use single quotes for URL
```yaml
#Sequances/Arrays/Lists  -- ordered
roles:
 - admin   # it also could be key: value
 - editor 
 - developer
# u can represent Lists using '[]'

# Multiline String 
description: >  # lines will be converted into spaces
 This is a folded multiline string.
 YAML removes newlines and converts it into a single long string.

description: |  # lines will be preserved
 This is a folded multiline string.
 YAML removes newlines and converts it into a single long string.
 
# complex keys  -> multi-line keys
? This is 
an example of a key
: "mostafa"

# Aliases/Anchors --> to easily duplicate content to reuse it again
anchored_content: &anchor_name This is a value that will be referenced again
use_reference: *anchor_name
--------------------
- step1: &id001
    instrument: Lasik 2000
    repetition: 1000
    spotSize: 1mm
- step2:
    <<: *id001 # use "<<" to duplicate an entire object(mapping or list)
    spotSize: 2mm # overwrite


extentions are .yaml or .yml

```