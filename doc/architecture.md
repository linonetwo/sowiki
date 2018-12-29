# Architecture

```diagram
SlateJSEditor <-> RematchStore <-> SoLiDSyncer
                               <-  SWFetcher
```

## SoLiDSyncer

When you enter a URI into SoWi's address bar, SoLiDSyncer will try to access this file from SoLiD POD.

If this file is a proper markdown, then it will be download to the RematchStore.

## RematchStore

There is a hash map in the store, caching all URI - content pair that user had requested. If there is a cache miss, RematchStore will call SWFetcher to load the content.

## SlateJSEditor

Load current markdown from RematchStore.

If there is an [embedding link](../README.md#Embedding), link content will be load from the RematchStore asynchronously.

## SWFetcher

Loading content from other site will encounter CORS issue, because browser is trying to prevent things like a vicious site embedding a bank site and cheating user.

But this can be bypassed utilizing service worker.
