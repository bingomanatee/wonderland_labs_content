## Saving your code == saving your life

I used to run the Mac labs back in the 80's and I saw several undergrads literally break out in tears when the work they'd been chewing on for a week was lost due to floppy disk failure. I myself left a whole box of disks on a friend's speaker and lost a year's worth of files. 

Nowadays, there are a range of options for saving your work. When it comes to coding or office files, Git/Github is hands down the best. However it is an **active** system -- unless you bundle and push a set of changes they are not preserved. 

Best practices are to make small changes and synchronize them with tests: 

1. change your code
2. write any tests needed to validate your code
3. run your tests to validate your new tests and prevent regression
4. commit your changes.

One of the problems with best practice, test-driven programming is that there is that uncomfortable state where you work is not passing tests but you want to back it up because you have reached some sort of "personal benchmark" -- or you are tired and want to go to bed. What do you do to preserve your work? You don't want to push to your branch because you know your tests aren't passing, right?

The evil devilspawn that is the modern hard drive is part of the problem. I don't trust it. I have about eight of them I've backed stuff up on and they are all failed at this point. The only thing a HD is good for is to enable a Time Machine (an OSX backup system built in to the operating system) and that isn't my favorite pet at this point either -- it is very poorly designed in that you can't exclude apps which nowadays are the least important files in your computer yet take up a lot of the space. 

So when it comes to saving your work without polluting the Git repos with bad code what are your options?

## 1. A Secondary Backup System

Using not-git to save your work has its pros and cons. Git is of course *versioned* so you can save the current state of your code AND see the history of updates back to day one. Most other backup systems don't have this advantage. 

Here are the qualities I like in a backup engine:

1. **passive** -- Its Murphy's law that any system that forces you to take action to back up your files results in your forgetting to back them up right when you are "hot" in a work run -- and your work is most likely to need backup!
2. **versioned** -- My great hatred of most DB systems and backup engines is they only save the latest state. One of the most common scenarios of backup retrieval is when you code down a bad road and want to take the path not taken. Saving the latest version doesn't give you that option. 
3. **remote** -- Saving to your computer has two great problems: you can't "Machine hop" or share your code over the web, and you risk losing work if your computer HD (or computer) is lost, stolen or damaged. Remote systems give you the most options. Also note most remote systems are "Pigs" -- they don't respect .gitignore and will gleeflully clog themselves with large media files.
4. **branch friendly** -- no system passes this test; ideally you want to view your code back in time, *relative to each branch.* The only real way to keep this standard is to use git-based backups. 

* **Time Machine** is the only *passive*, *versioned*, *remote* backup system I know of. One of the downsides, though, is that IT decides when to back up your files. 
* **JetBrains IDEs** is *passive* and *versioned* but not *remote*. However it it s great saver of recent updates and allows you to view whole project state at any given point. Besides its lack of remoting /branch friendly history it is the most fantastic backup system imaginable. 
* **DropBox** is *passive* and *remote*. and if you don't get piggish with tons of resource files, freeish. But it is definitely not versioned. Also it requires your work to be contained within a parent dropbox folder, which is sometimes a bit inconvenient -- you may for instance end up dropboxing your project AND all it's .git files which is kind of overkill; also the stuff you exclude with git `.ignore` will still clog your DropBox. 
* **(various web backup services)** There are a ton of web backup services. 
* **rsync** -- is *passive* and *remote* and (relatively) secure. Rsync is a unix tool that lets you keep folders synchronized (backed up) across the net. This is the only way to completely control your content yet still have a remoted backup system in place. The down side is that you may find it more expensive and definitely more complex to maintain a rsync-based backup.

### Remote Security

In today's troubled times, there is no such thing as a guarantee of privacy for backup systems -- in fact there is an almost guarantee that your data WILL be shared with a remote backup system. I don't have any great fix in this area, just a warning.

## 2. Intelligent use of Git

For any important branch you can maintain a "backup" branch; for instance, for a branch "adding_i18n" you can keep a branch "adding_i18n_backup" and save your work to that branch from time to time. This is of course NOT *passive* by any means. It is *remote* if you save it to GitHub; you can even save it to a separate repo for privacy, and to keep your main branch clean. 

This is a very active system -- you have to both juggle branches AND keep pushing to remotes for it to work -- and for that reason, can be distracting and unreliable. But it kind of qualifies as "Best practices" as it is *versioned* and *branch friendly*. 

## 3. Fuck it

Sometimes its fair to say, "This is a branch, my branch, and I'm going to push junk from time to time. Live with it." This takes less concentration than 2, and is a fair assumption to make. 

## 4. Mix the above

Blending a remote backup system with some sort of Git based backup system gives you the best safety modern computing allows.

# MMMMM. Thats GOOOOOOD Dogfood

So how do I back up my articles?

Each time I edit an article it's saved as a text file. So I have a versioned history of my articles. 

They are copied into a github repo. However I have not yet automated the process of publishing them to Github -- I have to ssh to my Wonderland Labs' server to do that. But soon.... 

Note, my articles never touch a database. They are too important :D. And databases are not inherently versioned. And they have a lot of moving parts. Github is in most ways the perfect database for highly important data.