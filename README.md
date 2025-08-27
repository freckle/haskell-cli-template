# haskell-cli-template

Haskell CLI template used at Freckle.

## Create your repo

If you are working within the freckle org, use github-vending-machine. Otherwise:
```sh
gh repo create --template freckle/haskell-cli-template --public freckle/<name>
```

## Rename your package

```sh
sed -i s/haskell-cli-template/my-name/ ./**/*
```

## Release

To trigger a new release, push a [conventional commit] to `main`:

- `fix:` to trigger a patch release
- `feat:` to trigger a minor release
- Use `<type>!:` or include a `BREAKING CHANGE: <change>` footer to trigger
  major

[conventional commit]: https://www.conventionalcommits.org/en/v1.0.0/#summary

---

[CHANGELOG](./CHANGELOG.md) | [LICENSE](./LICENSE)
