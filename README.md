# GitHub action: checkout

It's based on [Base Checkout Action](https://github.com/actions/checkout) and [Cached LFS Checkout Action](https://github.com/nschloe/action-cached-lfs-checkout).

We added a special reference handling called "alt_ref". This feature we need in case of a multi-repo build chain in case of mirror branches. If the feature branch does not exist in your repository, the alternative reference 'alt_ref' will be taken. To handle the reference stuff we are using [PyGithub](https://github.com/PyGithub/PyGithub).

You can set regular expression to checkout reference, too.

Check this out on [Github Marketplace](https://github.com/marketplace/actions/checkout-repo).

Hint(s):
- Actual you use only for once the action due to [this](https://github.community/t/sharing-environment-variables-between-steps-in-an-action-yml-wrong-value-in-case-of-mulitply-call/248736) bug. 
- Windows did not use an own virtual environment (python)
- This works only for GitHub Repos
- local files will be checked out to folder `.temp`

## Options

This action supports:
- checkout to a special folder / path
- submodules
- git lfs (in cached mode)
- select a special reference
- select a alternative reference (used when your regular reference does not exist)
- select a reference with regular expression e.g like \*/release/\*.\*.\* (and get the last matching, in case of downgrade you get the )

## Examples:

## Checkout a special branch
```yaml
- name: Checkout repo with a special branch
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    ref: my-branch
```

## Checkout private repo
```yaml
- name: Checkout private repo
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    repository: my-org/my-private-repo
    token: ${{ secrets.GH_PAT }} # `GH_PAT` is a secret that contains your PAT
    path: my-repo
```
> - ${{ github.token }} is scoped to the current repository, so if you want to checkout a different repository that is private you will need to provide your own PAT (personal access token). See [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) more details.

## GIT LFS repo
```yaml
- name: Checkout git lfs repo
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    lfs: true
```
This uses a regular checkout like [Cached LFS Checkout Action](https://github.com/nschloe/action-cached-lfs-checkout).

## Checkout repo with submodules
If you want to use LFS use:

```yaml
- name: Checkout repo with submodules
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    submodules: recursive
```
## Checkout a special branch with fallback alternative
```yaml
- name: Checkout repo with alternative ref
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    ref: feature/blue-light
    alt_ref: develop
```

## Checkout the last tag with a regular expression
```yaml
- name: Checkout repo with alternative ref
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    ref: */release/*.*.* 
```

## Checkout the last tag with a regular expression and not matching to default branch
```yaml
- name: Checkout repo with alternative ref
  uses: rohmanngmbh/action-checkout-repo@v1.3.2
  with:
    ref: */release/*.*.* 
    regex_next_to_last: true
```

### License

The scripts and documentation in this project are released under the MIT License.


### Support OS

[![Test](https://github.com/rohmanngmbh/action-checkout-repo/actions/workflows/ci.yml/badge.svg)](https://github.com/rohmanngmbh/action-checkout-repo/actions/workflows/ci.yml)

* ubuntu-20.04 (ubuntu-latest)
* ubuntu-18.04
* windows-2022 (windows-latest)
* windows-2019
* macos-12
* macos-11 (macos-latest)
* macos-10.15