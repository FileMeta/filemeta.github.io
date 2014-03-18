---
markdown: gfm
title: FileMeta Manifesto
layout: default
uuid: 0a2e5dd6-98eb-4b13-bfa5-b4c5787f497e
---
# The FileMeta Manifesto

The identity of a digital file should be a feature of the file. This principle may seem self-evident but it's not the way digital devices presently operate. Today, the identity of a file is the responsibility of the file system, database, or document management system in which the file is stored. Thus, the identity of a file changes when it moves between systems – from a computer, to the cloud, to a personal device, across email, and so forth.

Making identity a feature of the file helps keep files from being lost in a morass of nested subdirectories. Instead, identity persists across time, device and collection. As personal content moves into the cloud with synchronization to multiple devices, preservation of identity becomes increasingly important.

The FileMeta philosophy – that identity belongs in the file – affects the design of applications, operating systems and personal devices. Usability testing and focus groups are already guiding change in this direction. File systems are routinely indexed, "Tagging" is a common meme for internet applications. Music files such as MP3 and AAC are accessed according to attributes like genre, artist, and album. Conventional file systems are hidden from users on consumer devices like phones and tablets because they are deemed too difficult to use.

Applications that incorporate the following FileMeta principles will be more intuitive, will help people keep track of their work, support automated file management, protect against data loss, and facilitate secure and robust synchronization:

* __File identity belongs in the file.__ This way the file and it's identity are always together. They move together, they are stored together, they are updated together and they are indexed together. It also allows the identity to be much richer than a simple filename.

* __File identity should be part of the metadata.__  To date, the only consumer use of file metadata is music files; but there it's pervasive. Even tiny mp3 players index the files by genre, artist, album, track – thereby facilitating selection in collections including tens of thousands of files. Most contemporary file formats already include include metadata sections though they go underused.

* __Revolutionary changes occur incrementally.__ FileMeta principles can be layered into existing operating systems and applications without wholesale rejection of filenames and nested directories. Filesystem indexes are one example. Media players are another. Someday, perhaps, a new operating system will emerge that relies exclusively on file contents for identity. Until then, we'll live in a hybrid world.

* __File type is part of identity.__ Applications shouldn't rely on the filename extension that's stored externally to the file (at least, not exclusively). Conveniently, most existing file types have identifying signatures. In other cases, such as source code files, file type and metadata can be added to the contents by following certain conventions.
  
* __Identity includes an ID as well as a name.__ As with naming children and pets, choosing unique names is more troublesome than beneficial. But there does need to be a way to distinguish between things with the same name. So a unique ID is an important part of the identity. [UUIDs](http://en.wikipedia.org/wiki/Uuid) are best because they are globally unique.

* __Files should be self-describing.__ All information to be stored about a file, such as its origin, authors, dependencies, purpose, creation date, or modification history, belongs in the metadata of the file itself. Rather than create manifests or inventories, consider building the information into the file's own metadata.

* __Metadata is the master, indexes are for performance.__ Directories, rosters, and manifests are all indexes that speed up locating and accessing data. But indexes are only copies of data – the original belongs in the file itself.

* __Version history facilitates synchronization and merging.__ With cloud storage and occasionally-connected devices, file synchronization will continue to increase in importance. Setting version numbers and tracking version history facilitates synchronization and automated merging of changes.

The [FileMeta Project](http://www.filemeta.org) promotes these principles by reporting on related work, documenting metadata formats, collecting file type signatures, sharing best practices, and sharing source code to metadata tools. 