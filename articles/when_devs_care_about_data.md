Every time I go to a presentation of a new repository technology I keep seeing it as a contrast with the way they treat their own data.

By which I mean, their own source files. 

There was a long dark period where source files were hauled around on CDs and hard drives. Pretty soon, [CVS](http://en.wikipedia.org/wiki/Concurrent_Versions_System) (Concurrent Versioning System, 2008), [SVN](http://en.wikipedia.org/wiki/Subversion) (SubVersion) and [Git](http://en.wikipedia.org/wiki/Git) (Git, Github) came along to take the flop sweat out of project source control. 

I ran computer labs in college and almost every day, someone would burst a gasket when they found out that they'd lost days of work due to media failure of a floppy disk. I once lost a whole shelf of floppies by putting them too close to a speaker. (fair warning! giant speakers have a hefty EM signature.)

Now, I could lose a whole computer and be up to speed in a few days, thanks to a combination of cloud technology, Open Source and Github.  

But it does throw an interesting contrast to the level of integrity we expect of repos. 

### Versioning

I still find it pretty amazing that we don't demand our repositories provide versioning of updated data. Contrast this with Git/Github, where you never really overwrite file/data -- you provide updates, which compact to line-by-line revisions. 

By contrast, people overwrite, update, and delete rows of data with amazing abandon. 

To wit, the articles in this Blog are backed by a Github repo; every update is written as a new file, and the last version is saved in a `.backups` folder; the whole package is an independent repo. 

### Commit Integrity and [Concurrency Control](http://en.wikipedia.org/wiki/Multiversion_concurrency_control)

The concept of "checking in" or "Checking out" data is very haphazard; to my knowledge, only Couch enforces MVCC at the repo level. For the most part, the responsibility of ensuring that you are aware of changes in data state when you update it is completely put on the application developers. 

In Git, you are forced to reconcile your changes against subsequent activity between the point you check out a piece of data and the time you revise it. If you don't manually manage any conflicts, you can't (automatically) update it. 

### Deleting Data

It takes a huge amount of effort to permanently scrub all traces of a file in Git. 

By contrast, it takes no effort at all to do this in every Repository technology I can think of. 

## Excuses, excuses

There are a bunch of reasons that people accept this level of irresponsibility in Repository design. 

* "It would take too much space to do this routinely."
* "That's up to the application design"
* "It would slow down the repository too much."

All of these basically are intellectual ways of saying that *Data integrity is not the primary concern of my repository system.*. Imagine what would happen if Github suddenly decided that they weren't going to keep backups of previous commits because it took up too much space, or that they were going to routinely accept commits regardless of conflicts in order to save CPU cycles. 

# The Three Laws of Data

I would like to suggest a "Three Laws" approach to data management. 

1. **A repository should preserve all its data above all else.** This includes past entries, deleted records, etc. It should be very difficult, if not impossible, to &quot;Hard delete&quot; data.  
This should include routine and easy network distributed backups. 
2. **A repository should always flag commit conflicts.** or, **All data should be transactional.** Transactions were added to databases relatively late in the game, and is largely ignored in many noSQL systems. (CouchDB is a notable exception.) 
The emphasis has been on ensuring that a set of parallel modifications should only succeed if each and every one of them succeed. However, git added a twist in tracking whether or not your data was modified prior to  It should not be possible to update data that someone else has updated in the meantime. This should trigger an error that you must manually resolve. 
3. **Data integrity is more important than performance.** Mongo has taken a lot of (well deserved) hits in the past for allowing you to save data without waiting to see if it has been successfully committed to the repository, in order to inflate the responsiveness of the repository. 
This also brings up the issue of schema. NoSQL often flaunts its lack of enforced data types, and again, is often used as a way to back out of the responsibility on the part of a repo to enforce data integrity upon itself. (Mongoose.js takes up the challenge on the client level, though, and Couch does use schema as an optional feature in Couchbase (or core Couch?).
I do think there will always be  a place for a specific field to have an open/undefined schema but that should be a specific flag, rather than a default assumption.