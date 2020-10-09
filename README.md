# Maven Multi-module Project

I recently ran into an issue where a Maven multi-module project with many sub modules
was requiring numerous updates to the `pom.xml` files to reflect even minor version changes.

Here is a _Parent_ project that has 2 main sub trees, each with potentially different version roots.

Each module has numerous sub modules that all, typically, get versioned the same as the root submodule.
However, it also let's you address bugs and provide a minor revision down at the lowest level.

```
Parent                      1.0.0-RELEASE
 | - /applications          1.1.0-SNAPSHOT
 |   - /processor
 |     - /processor-1
 |       - pom.xml          1.0.1-RELEASE
 |     - /processor-2
 |       - pom.xml
 |     - pom.xml
 |   - /sink
 |     - /sink-1
 |       - pom.xml
 |     - /sink-2
 |       - pom.xml
 |     - pom.xml
 |   - /source
 |     - /source-1
 |       - pom.xml
 |     - /source-2
 |       - pom.xml
 |     - pom.xml
 |   - pom.xml
 | - functions              0.0.2-SNAPSHOT
 |   - /consumer
 |     - /consumer-1
 |       - pom.xml
 |     - pom.xml
 |   - /supplier
 |     - /supplier-1
 |       - pom.xml
 |     - pom.xml
```

Unless noted, versions propagate down the to the lowest level using the Maven
feature `${revision}`. You specify this as a property. It can now be used to 
reference the version of the parent in the `<parent/>` block of each sub module pom.

If, in fact, you need to address a bug change in a module, you can override the version
at that level only, thus minimizing the touch points of version maintenance.  