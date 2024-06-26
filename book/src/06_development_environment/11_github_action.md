# GitHub Action

The official GitHub action to download a prebuilt binary of Veryl is provided.

[https://github.com/marketplace/actions/setup-veryl](https://github.com/marketplace/actions/setup-veryl)

The examples of GitHub action script are below:

* Format and build check

```yaml
name: Check
on: [push, pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: veryl-lang/setup-veryl@v1
    - run: veryl fmt --check
    - run: veryl check
```

* Publish document through GitHub Pages

```yaml
name: Deploy
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: veryl-lang/setup-veryl@v1
    - run: veryl doc
    - uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: doc
```

* Test by [Verilator](https://www.veripool.org/verilator/)

For this purpose, we provide GitHub action [veryl-lang/setup-verilator](https://github.com/marketplace/actions/setup-verilator).

```yaml
name: Test
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-22.04
    steps:
    - uses: actions/checkout@v4
    - uses: veryl-lang/setup-veryl@v1
    - uses: veryl-lang/setup-verilator@v1
    - run: veryl test --sim verilator
```
