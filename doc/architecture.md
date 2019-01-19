# Architecture

```diagram
SlateJSEditor <-> RematchStore <-> SoLiDSyncer <-> SoLiD POD <-> Service
```

## SoLiDSyncer

When you enter a URI into SoWi's address bar, SoLiDSyncer will try to access this file from SoLiD POD.

If this file is a proper markdown, then it will be download to the RematchStore.

## RematchStore

There is a hash map in the store, caching all URI - content pair that user had requested. If there is a cache miss, RematchStore will call SWFetcher to load the content.

## SlateJSEditor

Load current markdown from RematchStore.

If there is an [embedding link](../README.md#Embedding), link content will be load from the RematchStore asynchronously.

## SoLiD POD

Files and their metadata are stored on your SoLiD POD, following LDP specification.

Some example is [here](./files.md).

## Service

SoLiD POD only provides storage, so hard jobs like searching and NLP parsing can only be done in third party Services.

Services have their WebID, if you want to use a service, you should [trust it in ACL](https://github.com/solid/web-access-control-spec#other-ideas-about-specifying-trusted-apps).

Services use [solid-auth-cli](https://github.com/jeff-zucker/solid-auth-cli) to login their WebID, and act as semantic web agent on the server-side.
