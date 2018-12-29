# SoWi

SoLiD Distributed (Federated) Wiki that will let user own their data.

## Introduction

This Wiki App is inspired by [Federated-Wiki](https://github.com/fedwiki).

Aiming to be serverless, SoWi store your editing in the browser for Offline use, and sync to your own SoLiD POD when you are Online.

You can use any other SoLiD App to view the raw data produced by this App, or export them at any time.

## Collaboration

You can fork other people's plain text or markdown file to your wiki, as long as those files are also on a SoLiD POD:

1. Put file's URI into SoWi's address bar (This will load the file if you pass the WebACL check of that file)
1. Click the "fork" button (This will make a copy of that file, and save it to your POD)

After some modification, you can contribute back to the source:

1. Click the "push" button (This will send a notification to source POD's Inbox)
1. Owner of the source POD will receive a notification with a button "accept"
1. By clicking "accept" button, source POD merge your file with its file

## Embedding

In markdown, if there is a link with a `(embed)` at the end of alt text:

```markdown
[Google's Homepage](https://www.google.com "Google's Homepage (embed)")
```

SoWi will try to parse the Content-Type of Links in the file, and embed the Content this link pointed to.

## Contribute

This App uses following packages:

- [localForage](https://github.com/localForage/localForage) (for Offline use)
- [solid-file-client](https://github.com/jeff-zucker/solid-file-client) (sync to SoLiD POD)
- SlateJS and React and Styled-Components (build rich text editor)

You don't have to know them all to contribute, read the [Architecture](./doc/architecture.md) document first, then open an issue to share your ideas.
