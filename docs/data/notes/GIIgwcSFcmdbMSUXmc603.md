
AppleScript is a scripting language for doing inter-application communication (IAC) using Apple events (ex. open a file, save a file). Most often, these actions are synchronous.

Whereas [[Apple events|mac.apple-events]] are a way to send messages into applications, AppleScript is a particular language designed to send Apple events
- the AppleScript language is designed on the natural language metaphor

AppleScript can send and receive Apple events to applications, and can act as a connector between different apps.

AppleScript relies on the functionality of applications and processes to handle complex tasks

AppleScript can be compared to a Unix shell in terms of its purpose

While not all apps are considered scriptable, any app with a graphical user interface responds to Apple Events at a minimal level. This is because OS X uses Apple Events to instruct all apps to perform core tasks such as launching, quitting, opening a document, and printing

A handler in AppleScript is equivalent to a function/method in Javascript

The heart of the AppleScript language is the use of terms that act as nouns and verbs that can be combined. For example, rather than a different verb to print a page, document or range of pages (such as printPage, printDocument, printRange), AppleScript uses a single "print" verb which can be combined with an object, such as a page, a document or a range of pages.
```
print page 1
print document 2
print pages 1 thru 5 of document 2
```

### Example Simple Web Gallery
1. Open a photo in a photo-editing application (by sending that application an Open File Apple event).
2. Tell the photo-editing application to reduce the resolution of the image
3. Tell the photo-editing application to save the changed image in a file in some different folder (by sending that application a Save and/or Close Apple event).
4. Send the new file path (via another Apple event) to a text editor or web editor application
5. Tell that editor application to write a link for the photo into an HTML file.
6. Repeat the above steps for an entire folder of images (hundreds or even thousands of photos).
7. Upload the HTML file and folder of revised photos to a website, by sending Apple events to a graphical FTP client, by using built-in AppleScript commands, or by sending Apple events to Unix FTP utilities.

# E Resources
[Good guide that goes over Mac Scripting](https://developer.apple.com/library/archive/documentation/LanguagesUtilities/Conceptual/MacAutomationScriptingGuide/index.html#//apple_ref/doc/uid/TP40016239-CH56-SW1)
