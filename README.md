## Installation Notes
The keep Python3 script will require at least the following modules.  Three are packaged at pipy.org and may be installed system-wide or via your preferred method, for example:

'pip3 install gkeepapi gpsoauth dateparser`

The goopy repository should be placed somewhere within PYTHONPATH:

`git clone git@github.com:ElectricBill/goopy`

### Configuration
The program requires login credentials to access your Google Keep notes. It will display instructions for obtaining and credentials and setting up the configuration file when first run with no arguments.

## Usage

### Action: select
The select action provides a way to identify subsets of notes in a collection based on attributes and apply operations to them.  The collection can be one of either Active, Archived or Trashed notes.  The selection criteria can be combinations of:
#### Filters
- the color(s) of the notes
- the labels attached to the notes
- the presence of a particular string in the text
- the presence of a string that matches a specified regular expression
- timestamp ranges of the notes:
  - created time
  - updated time
  - edited time
  - archived time
  - trashed time
- exclude pinned notes
- include only pinned notes

##### --{cueat}time ''specification''
Time filter specifications are combinations of ''{before|after} TIME'' as compared to the particular timestamp.  ''TIME'' is given in any format acceptable by the '''dateparser''' module.  The ''after'' comparator is inclusive and the ''before'' comparator is exclusive.

##### Operations
The operations that may be applied are:

Options to modify notes:

- add label(s) (--tag)
- remove label(s) (--untag)
- set the color (--color)
- pin the note (--pin)
- unpin the note (--unpin)
- move to Archive (--archive)
- move to Trash (--trash)
- move to Active from Archive or Trash (--revive)

Options to operate with notes:

- pipe the note content (not title) through a shell (--exec)
- extract each note into a separate file at a designated path (--extract)

#### --extract
If the option is given with no argument it defaults to "keepnotes". A directory with this name is created.  It must not exist prior.  The extracted notes are store in subdirectories: main, Archive and Trash in JSON format.  Addition, subdirectories are created for each unique label found attached to extracted notes and a hardlink to the file for each note so tagged with that label is created in the correspoding label subdirectory.  Additionally objects in notes auch as images are stored in a subdirectory named blob.
### Action: note
This action creates a new note.  The --labels option may be use to set labels, similar to the way the --tag option is used with the select action. The new note will be pinnned and set to color Blue.

### Reporting Level --verbosity
Defaults to 10.

- Level 5 or higher will show summary tallies of selected notes.
- Level 10 will list individual selected note summary data.
- Level 20 will also list selected note content.

## Usage Examples

Display all notes:

`keep select`

Display all notes that are red in color:

`keep select -c red`

Add label "recent" to all notes created within 1 week of yesterday:

`keep select -l --ctime after 'yesterday - 1 week' --tag recent`

Create a new note tagged with *fresh* label and with a title and two lines of content:

`keep note --labels fresh --text 'remember this' 'OK, Charlie?' 'second line'`
