# Backblaze Excludes
[BackBlaze](https://www.backblaze.com/) (BB) is great for continuous cloud backups. It's an excellent solution for 
almost everyone, but if you're a developer using tools like Node.js, maven or pyenv, BB's "backup everything" policy 
is too inclusive. BB preferences app makes it easy to exclude normal/visible directories, but sadly hidden directories 
are... well, hidden and can't be added to the list of directories to exclude. That means hidden directories like 
~/.npm, ~/.m2 and ~/.pyenv are always included in BB backups Doing so increases the
size and number of files sent to the cloud and leads to slow backups. Slow backups may not be a problem for you, but it
is for those of us stuck with crappy ADSL2 connections at home...

At the time of writing, the only way to exclude hidden directories from BB backups is to 
[Set Custom Exclusions](https://help.backblaze.com/hc/en-us/articles/220973007-Advanced-Topic-Setting-Custom-Exclusions-via-XML)
. Assuming BB is installed in the default location on MacOS, add XML rules like the following to /Library/Backblaze.bzpkg/bzdata/bzexcluderules_editable.xml:

```
    <excludefname_rule plat="mac" osVers="*"  ruleIsOptional="t" skipFirstCharThenStartsWith="users/" contains_1="/.m2/" contains_2="*" doesNotContain="*" endsWith="*" hasFileExtension="*" />
    <excludefname_rule plat="mac" osVers="*"  ruleIsOptional="t" skipFirstCharThenStartsWith="users/" contains_1="/.npm/" contains_2="*" doesNotContain="*" endsWith="*" hasFileExtension="*" />
    <excludefname_rule plat="mac" osVers="*"  ruleIsOptional="t" skipFirstCharThenStartsWith="users/" contains_1="/.nvm/" contains_2="*" doesNotContain="*" endsWith="*" hasFileExtension="*" />
    <excludefname_rule plat="mac" osVers="*"  ruleIsOptional="t" skipFirstCharThenStartsWith="users/" contains_1="/.pyenv/" contains_2="*" doesNotContain="*" endsWith="*" hasFileExtension="*" />
    <excludefname_rule plat="mac" osVers="*"  ruleIsOptional="t" skipFirstCharThenStartsWith="users/" contains_1="/.rbenv/" contains_2="*" doesNotContain="*" endsWith="*" hasFileExtension="*" />
```
Note:
* Make sure the file remains a valid XML document - adding bew excludefname_rule elements above ```</bzexclusions>``` 
  is a good idea.
* All excludefname_rule attributes must be included ('*' means ignore).
* Test the change by opening BB Preferences -> Settings -> Reports and selecting Files Scheduled for Backup and 
  confirming the directory to be excluded doesn't appear in the list of files.
* Make sure you spell the directory names correctly, otherwise you spend too much time wondering why the file list 
  isn't updated...