# Architecture

```diagram
SlateJSEditor <-> RematchStore <-> SoLiDSyncer <-> SoLiD POD <-> SoLiD Service
```

## SoLiDSyncer

When you enter a URI into SoWi's address bar, SoLiDSyncer will try to access this file from SoLiD POD.

If this file is a proper markdown, then it will be download to the RematchStore.

## RematchStore

There is a hash map in the store, caching all URI-content-pair that user had requested. If there is a cache miss, RematchStore will call SoLiDSyncer to load the content.

## SlateJSEditor

Load current markdown from RematchStore or embed as an IFrame.

If there is an [embedding link from a different origin](../README.md#Embedding), link content will be loaded as an Iframe.

If there is an [embedding link from same origin](../README.md#Embedding), link content will be loaded from RematchStore, and a custom View will be provided.

### View

CSV file can be regarded as a spreadsheet or a list, even a kanban.

So when loading a CSV file for user, user can choose a View that best represent the current data set.

Views are basically React components, that take the file content in, and returns a component tree. And of course, they have access to some updating functions, so you can edit the data set seamlessly.

### View Plugins

You can load React components at runtime. Following our plugin specification, you can wrap your React component into a SoWiki View Plugin, that can be discovered and installed from the Plugin Marketplace.

Note that all buttons and Nav on SoWiki are just Views, so you can customize them by installing or creating a View plugin.

## SoLiD POD

Files and their metadata are stored on your SoLiD POD, following LDP specification.

Some example is [here](./files.md).

## SoLiD Service

SoLiD POD only provides storage, so hard jobs like searching and NLP parsing can only be done in third party Services.

Services have their WebID, if you want to use a service, you should [trust it in ACL](https://github.com/solid/web-access-control-spec#other-ideas-about-specifying-trusted-apps).

Services use [solid-auth-cli](https://github.com/jeff-zucker/solid-auth-cli) to login their WebID, and act as semantic web agent on the server-side.

### Crawler

Some service can help you sync your current social network account data to solid, likes visual crawler [portia](https://portia.readthedocs.io/en/latest/getting-started.html) or JS based crawler [apify](https://www.apify.com/).

Or you can subscribe to a Reddit channel, subscribe to micro-blog that is about new npm package, and so on. Results are send to your POD, and forever owned by yourself.

### Custom Script Executor

You may want to update your document on a regular basis, for example collect documents that's tagged as `#todo` to a single CSV data set, so you can view them as a kanban or calendar.

To do such a custom job, you can write an Extension Script, and make it an SoLiD Service.

You can share your Extension Script to the Extension Marketplace, so other people can deploy their own service, make use of your automated workflow. Or fork your script and make some change to suit their needs.
