# Jenkins pipeline helpers

This repository contains some simple helper functions, which can be used to
automate GitHub release handling throughout [Jenkins-CI](https://jenkins.io/).
They need to be included as [Shared Library](https://jenkins.io/doc/book/pipeline/shared-libraries/).

## Git tag handling

### createTag(...)

This function creates a Git tag with given name on given commit with
given comment as commit message. This is the first step to create a release
on GitHub because each release is corresponding to a Git tag.

The used account must have push permission.

Usage:
```
createTag(
    tag: "<tag-name>",
    commit: "<hash-or-HEAD>",
    comment: "<comment-for-commit-message>"
)
```

Optional parameters:
* *comment:* - If not given, the value from *tag:* will be used as comment.

## GitHub release handling

The whole GitHub release handling is based on the Docker image
[github-release-uploader](https://github.com/starwarsfan/github-release-uploader).
So the functions here are more or less wrappers around the different Docker-run-calls
with initial parameter validation and error handling.

### createRelease(...)

This function creates a GitHub release on the given Git tag.

If the value given with *description:* is the name of an existing file,
it's content will be used as the release description. This file should
contain the release notes, which might be formatted using GitHub markdown
syntax.

Usage:
```
createRelease(
    user: '<github-project>',
    repository: '<github-repository>',
    tag: "<git-tag>",
    name: "<release-name>",
    description: "<release-description-or-release-notes-file>",
    preRelease: true|false
)
```

Optional parameters:
* *tag:* - If not given, *latest* will be used.
* *name:* / *description* - If not given, the value from *tag:* will be used.
* *preRelease:* - If not given, *false*

### isReleaseExisting(...)

This functions checks if a release is existing on the given Git tag. If yes,
*true* is returned, otherwise *false*.

Usage:
```
isReleaseExisting(
    user: '<github-project>',
    repository: '<github-repository>',
    tag: "<git-tag>",
)
```

Optional parameters:
* *tag:* - If not given, *latest* will be used.

### removeRelease(...)

This function removes the release on the given Git tag.

Usage:
```
removeRelease(
    user: '<github-project>',
    repository: '<github-repository>',
    tag: "<git-tag>",
)
```

Optional parameters:
* *tag:* - If not given, *latest* will be used.

### updateRelease(...)

This function updates an already existing release on the given Git tag.
All attached artifacts will stay in place.

If the value given with *description:* is the name of an existing file,
it's content will be used as the release description. This file should
contain the release notes, which might be formatted using GitHub markdown
syntax.

Usage:
```
    user: '<github-project>',
    repository: '<github-repository>',
    tag: "<git-tag>",
    name: "<release-name>",
    description: "<release-description-or-release-notes-file>",
    preRelease: true|false
)
```

Optional parameters:
* *tag:* - If not given, *latest* will be used.
* *name:* / *description* - If not given, the value from *tag:* will be used.
* *preRelease:* - If not given, *false*

### uploadArtifactToGitHub(...)

This function can be used to upload artifacts to the GitHub release on
the given Git tag.

The values on *artifactNameLocal:* and *artifactNameRemote:* define the file
names of the artifact to upload and how it should be named after upload.

Usage:
```
uploadArtifactToGitHub(
    user: '<github-project>',
    repository: '<github-repository>',
    tag: "<git-tag>",
    artifactNameLocal: "<local-filename>",
    artifactNameRemote: "<filename-on-github>"
)
```

Optional parameters:
* *artifactNameLocal:* - If not given, the value from *artifactNameRemote:*
  will be used.

