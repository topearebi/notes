---
tags: [zettel, object]
type: concept
---
# 🧠 

**Related:** [[zettelkasten/]]

---
## The Concept


## References

I/Llew2
https://www.reddit.com/r/selfhosted/s/Byz3RRNM75

The Gray Box Problem of Self Hosting

A big draw of self hosting is the ability to control your own data.

However, I've repeatedly run into a problem in self-hosting which I think of as the Gray Box problem. To understand gray boxes, lets first look at black and white boxes.

# Black Box:

In a black box app, you neither possess or directly manage your files.

Your files live on someone else's hard drive, and you're denied access except via their UI.

When you upload your files to a provider (think: google), they effectively enter a black box: getting them out again is difficult, and it's impossible to interact with the raw files themselves - your only access is through their proprietary UI. If you are able to get them out of the Black Box via a takeout procedure, the metadata is often unreliable and the files have no innate organization.

In contract to a White Box:

# White Box:

In a white box program, your files live on your hard drive, and you can manage them directly. The program sits on top of your own folder structure, but provides all the additional benefits of a UI for organization and other features.

The critical White Box criteria: \*The program picks up changes made to your files both inside AND outside of itself.\*

The best example I know of is Digikam, the open source photo management software. It sits over top your photos, and you can organize photos/metadata through the program's UI, but it **also** picks up changes you make directly to the files themselves - changes not made through Digikam.

Another white box example is Obsidian. Although it's proprietary software and not open source, you barely notice because it's a white box program - it sits atop files on your hard drive, which you can edit freely, but adds incredible management benefits when you use the UI.

# Gray Box:

In a gray box application, your files live on your hard drive (or NAS), but management is restricted to the program's UI.

Example: Paperless-ngx.

You can upload your files to Paperless, but if you change, move or edit the files outside of the UI, you will break it.

NOTE: Custom Storage Paths do NOT make an application into a white box program. Simply accessing them in a human readable format is not enough: you must be able to edit them freely outside of the program's UI, and have the program accept those changes without breaking.

This is the issue I keep wrestling with:

We're in the digital age now: your files will belong to you for a lifetime. When a program locks your files into a black or even gray box, it's guaranteed to be a short term solution - one day, you will have to recover your files from this program, whether it's self hosted or not.

Better to have an organization system for your own files and folders (whatever that looks like), that a program non-destructively accepts and works with/hosts, than to lock your files into any kind of short term box.

**Borderline cases:**

A borderline program is Immich: intrinsically it's a gray box program - if you externally touch photos that have been uploaded to it, both you and Immich are totally screwed.

But it has the saving grace of accepting external libraries, which means it can function as a white box program. The one feature that would make Immich truly white-box is if it wrote metadata to the photos themselves (as much as possible), instead of keeping it all in a database. There are some write-back workarounds for this people are making, but it's not native.

**Personal case:**

After years of working on it, I finally came up with a personal organizational system that works for me. I know where to find anything I need - files, photos, media - on my computer.

I wanted to up the anti last year by self hosting my files for mobile access. However, I started running into gray box issues - many programs demand I sacrifice my hard-won organizational structure for the modest convenience of a custom UI and tagging features.

This post is my attempt to think through the issue.
