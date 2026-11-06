# zovo-vsce-publish

Publishes prebuilt `.vsix` files for the Marketplace publisher `zovo` from GitHub Actions with
[trusted publishing](https://code.visualstudio.com/api/working-with-extensions/publishing-extension) (OIDC). No personal access token is stored anywhere.

How it works:

1. A release in this repo carries one `.vsix`. Its tag is `<extension name>-v<version>`, for example `tensor-viewer-v0.1.4`.
2. `.github/workflows/publish.yml` runs only when started by hand (`workflow_dispatch`) with that tag. It checks the publisher, name and version, then runs `vsce publish --oidc --packagePath <vsix> --skip-duplicate`.
3. The Marketplace trusted publishing policy on publisher `zovo` allows this repo and the workflow file `publish.yml`. Renaming either one stops publishing until the policy is changed.

Start one by hand:

```sh
gh release create tensor-viewer-v0.1.4 dist/tensor-viewer-0.1.4.vsix --repo theluckystrike/zovo-vsce-publish --title "zovo.tensor-viewer 0.1.4" --notes "VSIX"
gh workflow run publish.yml --repo theluckystrike/zovo-vsce-publish -f release=tensor-viewer-v0.1.4
```

The factory script `run-20261106/wave8/scripts/publish_all.sh` does both steps for a list of files, waits for each run and leaves 60 seconds between files.
