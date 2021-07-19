# terrraform-plugin-installer
It's a script to install third party terraform plugins and providers! Hassle free! How neat is that?

Installs plugins as `terraform-provider-<NAME>_vX.Y.Z` to `~/.terraform.d/plugins`

See more on third party plugin installation here: https://www.terraform.io/docs/configuration/providers.html#third-party-plugins

## Requirements

1) [golang](https://golang.org/doc/install) (for `go build`)

## Features

* Install from any git repository (local or remote)
* Clones/builds in a temporary directory (using `mktemp`)
* Install any specified revision/version (branch, tag, or git sha)
* Install _as_ any version
  * You can even install a revision whose git tag is a valid semver as a completely different semver, though I'm not sure why you'd want to do this :)
* Quickly test plugins in development without worrying about tagging, semvers, etc
  * Behavior is sane even for terraform plugin git repositories with no branches or tags (defaults to installing `HEAD` as version 0.0.1)  

## Usage

```
./install.sh <repository_url_or_path> [ <revision/tag> ] [ <version> ]
```

Note:

  * If the `revision/tag` _**is**_ the version, supplying the `version` argument is not necessary
  * If no `revision/tag` is provided, the latest release matching the semver regex (or if none, `HEAD`) will be built and installed.


## Example

### Install a release

```
./install.sh github.com/samstav/terraform-provider-mailgunv3 v0.3.2
```

### Install version specified from git sha

```
# In this example, v0.3.2 can be any version you'd like to install 3fafde7597 as
./install.sh github.com/samstav/terraform-provider-mailgunv3 3fafde7597 v0.3.2
```


### Terraform configuration

```
...

provider "mailgunv3" {
  version = ">=0.3.2"
}

```

## If you like `curl`ing to `sh`

```
curl https://raw.githubusercontent.com/samstav/terraform-plugin-installer/main/install.sh | bash -s -- github.com/phillbaker/terraform-provider-mailgunv3
```
