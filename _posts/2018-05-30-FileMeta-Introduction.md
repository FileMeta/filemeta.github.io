---
title: What is FileMeta About?
date: 2018-05-30 18:00
labels: metadata;
author: Brandt Redd
---
<img style="float: right;" src="/postimg/FileMeta_Logo.jpg" alt="FileMeta Logo" width="210" />

We like to take pictures. My family's collection exceeds 100,000 digital photos. Over the years, we have successfully arranged them into a hierarchy of folders organized chronologically by year, month, and event. But the process is getting more challenging as we integrate photos from multiple cameras and phones plus pictures sent to us by others.

This got me pondering, "What is the identity of a file, and what should it be?"

On existing computer systems, the identity of a file is its filename; or, more correctly, its file path including all of the folders that lead up to the file. But there are serious problems with this:

- Filenames, and especially paths, are ephemeral. They change as we move files around.
- Hierarchical file systems make sense to librarians and computer scientists, but regular people have an easier time understanding hashtags.
- A filename is a property of where it's stored, not inherent in the file itself.

I propose that the identity of a file should be its metadata - the title, author, keywords, creation date, and so forth. And the metadata could be stored internally to the file so that it is preserved and carried along when the file is copied or transmitted. Thus the identity is inherent in the file, not in the place it's stored.

Conveniently, most contemporary file formats have a way to store the metadata. This includes Microsoft Office documents, PDF files, media files like MP3 and MP4 and many more. Current versions of Windows and Mac OS even retrieve and index the information.

The foundation has been laid for a metadata-centric way of managing our personal collections of pictures, documents, music, and video. What we lack are well-structured tools for tagging and retrieving. That will be the subject of this blog and the associated [FileMeta Project](http://www.filemeta.org).

I'm starting with better ways of managing our collection of photos. Instead of a single, chronological, dimension; metadata will let us organize the photos by subject, theme, people appearing in the photos, location, and subject. The experimental tools I write are open source and managed on [GitHub](https://www.github.com/FileMeta). And when some are polished well enough I'll post them in the Microsoft store.