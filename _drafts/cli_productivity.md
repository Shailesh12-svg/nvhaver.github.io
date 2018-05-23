# CLI Productivity Tracking with Moro

Moro is a free and open source NodeJS based application. Moro is Finnish for "Hello".

## Installation 

1. Install NodeJS
2. Install Moro from Npm: `npm install -g moro`

## Usage

Whenever you want to log a point in time, just issue the command `moro`. 
It takes a default break into account of 30 minutes. This can be changed as follows: `moro break 45`.
To permanently change the defaults:
`moro config --day 7` to set the length of a working day to 7 hours
`moro config --break 45` to set the break time to 45 minutes.

To get a report of all registered timestamps, issue the command `moro report --all`.
To make a correction to the start time: `moro hi 10:00`.
To make a correction to the end time: `moro bye 18:00`.
To clear all data, issue `moro clear --yes`.
A note can be added to a timestamp using `moro note mynotes`. These notes can later be used for searching: `moro search mynotes`

The data is stored in the home folder in the .moro-data.db file. To change the path of this file, issue: 
`moro config --database-path /home/documents/moro-backup.db`

The help summary can be obtained by issuing the command `moro --help`.
