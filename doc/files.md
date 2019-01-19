# Files

Files are stored on your SoLiD POD, following W3C's LDP specification, so they are seamless accessible to other services and can be exported easily.

## Container

This is basically same as folder/directory on your local file system, but a `Container` on the LDP is actually a RDF file looks like this:

```turtle
# somePodProvider.com/inbox/
@prefix : <#>.
@prefix inbox: <>.
@prefix ldp: <http://www.w3.org/ns/ldp#>.
@prefix terms: <http://purl.org/dc/terms/>.
@prefix XML: <http://www.w3.org/2001/XMLSchema#>.
@prefix n0: <adsf/>.
@prefix n1: <asdfdafsdfsa/>.
@prefix sub: <subscription/>.
@prefix st: <http://www.w3.org/ns/posix/stat#>.

inbox:
    a ldp:BasicContainer, ldp:Container;
    terms:modified "2019-01-16T15:43:13Z"^^XML:dateTime;
    ldp:contains n0:, n1:, sub:;
    st:mtime 1547653393.004;
    st:size 4096.
n0:
    a ldp:BasicContainer, ldp:Container, ldp:Resource;
    terms:modified "2019-01-16T14:25:18Z"^^XML:dateTime;
    st:mtime 1547648718.885;
    st:size 4096.
n1:
    a ldp:BasicContainer, ldp:Container, ldp:Resource;
    terms:modified "2019-01-16T15:43:13Z"^^XML:dateTime;
    st:mtime 1547653393.004;
    st:size 4096.
sub:
    a ldp:BasicContainer, ldp:Container, ldp:Resource;
    terms:modified "2019-01-16T14:14:37Z"^^XML:dateTime;
    st:mtime 1547648077.255;
    st:size 4096.
```

Note that URI of a container must have a tailing slash `xxx/`.

Let me explain each part of this container's turtle RDF file.

### prefix

`@prefix` defines shortcuts, so this file can be compact and clean to read.

`@prefix : <#>.` works likes a macro, transform `:this` to `<#this>`, and since relative URI is frequently used in LDP, this may save some typing and shorten the file.

`@prefix inbox: <>.` will transform `inbox:` to [null relative URI](https://www.w3.org/TR/ldp/#h-ldpc-post-rdfnullrel), which means current folder/resource or created resource (when you are creating a resource). So actually this empty URI is [the same as current base URI](https://www.w3.org/TR/turtle/#sec-iri-references).

### metadata of container

So metadata about current container can be writen as:

```turtle
inbox:
    a ldp:BasicContainer, ldp:Container;
    terms:modified "2019-01-16T15:43:13Z"^^XML:dateTime;
    ldp:contains n0:, n1:, sub:;
    st:mtime 1547653393.004;
    st:size 4096.
```

It describes that current URI has [type](https://www.w3.org/TR/2014/REC-turtle-20140225/#iri-a)(`a`) of `ldp:BasicContainer` and `ldp:Container`;

Second metadata is that this container is `terms:modified` at the `XML:dateTime` of `"2019-01-16T15:43:13Z"`;

Third metadata shows that this container has three files inside it, they are `n0:` and `n1:` and `sub:`.

### subFolder

And following prefix declaration are short-hands for container `adsf/` and `asdfdafsdfsa/` and `subscription/`:

```turtle
@prefix n0: <adsf/>.
@prefix n1: <asdfdafsdfsa/>.
@prefix sub: <subscription/>.
```

Then, the metadata of `subscription` (shortened as `sub`) shows that it is also a container:

```turtle
sub:
    a ldp:BasicContainer, ldp:Container, ldp:Resource;
```

Don't forget that containers are just RDF files, so let's look into how `subscription/` is implemented:

```turtle
# somePodProvider.com/inbox/subscription/
@prefix : <#>.
@prefix sub: <>.
@prefix ldp: <http://www.w3.org/ns/ldp#>.
@prefix terms: <http://purl.org/dc/terms/>.
@prefix XML: <http://www.w3.org/2001/XMLSchema#>.
@prefix st: <http://www.w3.org/ns/posix/stat#>.
@prefix m: <http://www.w3.org/ns/iana/media-types/text/markdown#>.

sub:
    a ldp:BasicContainer, ldp:Container;
    terms:modified "2019-01-16T14:14:37Z"^^XML:dateTime;
    ldp:contains <Readme.md>;
    st:mtime 1547648077.255;
    st:size 4096.
<Readme.md>
    a m:Resource, ldp:Resource;
    terms:modified "2019-01-19T04:20:02Z"^^XML:dateTime;
    st:mtime 1547871602.534;
    st:size 39.
```

Basically the same as how `inbox` is implemented.

But `@prefix inbox: <>.` has changed to `@prefix sub: <>.`, and metadata of container `subscription` changes as follow:

```diff
sub:
-    a ldp:BasicContainer, ldp:Container, ldp:Resource;
+    a ldp:BasicContainer, ldp:Container;
    terms:modified "2019-01-16T14:14:37Z"^^XML:dateTime;
+   ldp:contains Re:;
    st:mtime 1547648077.255;
    st:size 4096.
```

`ldp:Resource` means it is addressable using URI. When we are currently in `somePodProvider.com/inbox/subscription/`, it is already opened by it's URI, so no need to say it is `ldp:Resource` again.

And since wea re in this folder, we can see what this folder contains. In this example, we can see folder contains a file `somePodProvider.com/inbox/subscription/Readme.md`, which in relative URI (relative to somePodProvider.com/inbox/subscription/) is `<Readme.md>`.

### Resources

You can consider LDP as a net-disk, that can upload anything, for example a markdown file:

( `somePodProvider.com/inbox/subscription/Readme.md` )

```markdown
[google](www.google.com) is a website.
```
