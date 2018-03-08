svnt
====

... is a wrapper on SVN which lets you store the real modification times of files.


By itself, depending on the config, SVN will either trample the timestamps
outright or set them to the time the file in question was last committed to
the repository.  For many projects that's enough, but if you either want to
preserve a significant chunk of history prior to migration to SVN or want to
keep out-of-SVN timestamps for some reason, you're currently out of luck. 
Subversion guys promised to work on this problem, yet over a decade later,
not only there's no fix but SVN itself has became completely obsolete.

This is where svnt steps in.  It's nothing but a crude hack, though.

Bugs
====

 * it's insanely slow.  Forget about using this wrapper on any but tiny
   projects; it's probably not worth the time to rewrite it in a real
   language before there's an official fix.
 * property conflicts need to be resolved by hand
 * it doesn't try to handle "*merge*", "*export*" or "*import*"
 * timestamps are not updated immediately after a "*revert*", albeit any
   "*update*" or "*commit*" will, respectively, fix up or ignore them.
 * handling of empty vs non-empty files is counterintuitive 

Details
=======

The timestamp is stored as "file:timestamp" as number of seconds since
1970-01-01, with an optional (ignored) fractional part.

A non-empty file whose timestamp is changed but contents stay put will not
get committed, as this would lead to many unnecessary property changes in
many cases.  An empty file, though, is considered as a stamp of some sort
and thus will have its mtime heeded.  Directories are completely ignored.

Credits: plenty of code in svnt comes from Mark Ross' "asvn".
