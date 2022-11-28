# haskell-cli-template

Haskell CLI template used at Freckle.

## Create your repo

```sh
gh repo create --template freckle/haskell-cli-template --public freckle/<name>
```

## Rename your package

```sh
sed -i s/haskell-cli-template/my-name/ ./**/*
```

## Enable release

When you are ready to release your CLI, simply remove the conditional from the
release workflow.

```diff
-      - if: false # Remove when ready to release
```

---

[CHANGELOG](./CHANGELOG.md) | [LICENSE](./LICENSE)
