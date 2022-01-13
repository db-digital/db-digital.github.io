---
title: Kotlin Delegates
author:
  name: Mahesh Bhatt
  link: https://github.com/akamahesh-db
date: 2022-01-14 02:55:00 +0800
categories: [Blogging, Kotlin]
tags: [kotlin delegates, Kotlin Property delegation]
pin: true
---

## Delegation Pattern

In software engineering, the [Delegation Pattern][delegate-pattern] is an object-oriented design pattern that allows object `composition` to achieve the same code reuse as `inheritance`.

### Delegation

`Delegation` is the assignment of authority from one instance to another. It can operate mutable as well as static relationships between classes,  and inheritance, in turn , is based on the constant concept.

#### What does it mean? 

Suppose, we want to enable class B to have functionality of class A and condition `is a` is meet. Thus, you can use `inheritance​`. It will give you a permanent static relationship between these classes, and you will not be able to change it.

Using Delegation, you can pass an object of another type, such as subtype of class A, to instance of B. This fact makes Delegation an extremely powerful mechanism.

There are two types of delegation:
- Explicitly: Can be implemented in any object oriented language.
- Implicitly: Requires language support for this feature

#### Explicitly Delegation: 

It can be implemented in any OOP language. Let's look at the example:

Here We have a `MediaManager` which has two reposibilities downloading the file and play it, which is further delegated to the objects name `FileDownloader` and `FilePlayer`

FileDownloader.kt

```
class FileDownloader(private val fileName: String) : Downloader {

    override fun download() {
        println("Download File $fileName")
    }
}
```

FilePlayer.kt

```
class FilePlayer(private val fileName: String) : Player {

    override fun play() {
        println("Play File $fileName")
    }

}
```

FileManager.kt

```
class MediaFileManager(private val downloader: FileDownloader, private val player: FilePlayer) : Downloader, Player {
  
    override fun download() {
        downloader.download()
    }

    override fun play() {
        player.play()
    }

}
```

This has been mentioned previously: delegation pattern builds mutable relationship between classes, that’s why it’s a really flexible and powerful mechanism. 
As indicated in previous examples, Explicitly Delegation requires a lot of efforts to write methods that contain [Forwarding][forwarding] calls to delegate instance.


#### Implicitly Delegation 
Language-level feature that allows an object designate another one as its "parent" is called Implicitly Delegation. It, in particular, is divided into `unanticipted delegation` and `anitcipated delegation`.

When delegation structure can be changed dynamically, is called unanticipated,
whereas the second type refers to the fact that object can't change parent during their life-cycle.

`Kotlin`, unlike Java, supports delegation natively by language in the form of [Class-Delegation][class-delegation-kotlin] and [Delegated Properties][property-delegation]

Example `MediaManager.kt` in kotlin can be written as 

```

class MediaManager(private val downloader: FileDownloader, private val player: FilePlayer) : Downloader by downloader, Player by player

```





[class-delegation-kotlin]: https://kotlinlang.org/docs/delegation.html
[property-delegation]: https://kotlinlang.org/docs/delegated-properties.html
[delegate-pattern]: http://best-practice-software-engineering.ifs.tuwien.ac.at/patterns/delegation.html
[forwarding]: https://en.wikipedia.org/wiki/Forwarding_(object-oriented_programming)
