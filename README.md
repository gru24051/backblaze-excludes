# Backblaze Excludes
[BackBlaze](https://www.backblaze.com/) (BB) is great for continuous cloud backups. It's an excellent solution for 
almost everyone, but if you're a developer using tools like Node.js, maven or pyenv, BB's "backup everything" policy 
is too inclusive. By default BB will back up hidden directories like ~/.npm, ~/.m2 and ~/.pyenv. Doing so increases the
size and number of files sent to the cloud and leads to slow backups. Slow backups may not be a problem for you, but it
is for those of us stuck with crappy ADSL2 connections at home...

At the time of writing, the only way to exclude hidden directories from BB backups is to Set Custom Exclusions. Do this
by editing the file /Library/Backblaze.bzpkg/bzdata/bzexcluderules_editable.xml (assuming BB default MacOS installation) 
to add XML rules for each of the hidden directories to be excluded from the backup. Here are some examples:

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