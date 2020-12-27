* Creating a module

  * go mod init

* Retrieving dependencies

  * go get -u

* Maintaining a module

  * go list

  * 验证module

    * go mod verify

  * Clean dependencies

    ​	go mod tidy



## Understanding Module Versioning



### Semantic versioning

V1.5.3-pre1

V - version prefix (required)

1 - Major revision (likeley to break backward compatibility)

5 - Minor version (new features, doesn't break BC)

3 - Patch (bug fixes, no new features, and doesn't break BC)

pre1 - Pre-release of new version, if applicable (text is arbitrary)

### Module versioning rules

v2+

BC should be preserved within a major version

Each major version has unique import path

### Module queries





