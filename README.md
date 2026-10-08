# companion-module-globalcache-itac-ir

Bitfocus Companion module for Global Caché iTach IR interfaces (IP2IR, WF2IR).

See [HELP.md](./companion/HELP.md) for usage and [LICENSE](./LICENSE).

## Development

```sh
yarn install
yarn test      # unit tests, including failure scenarios against a simulated iTach
yarn lint
yarn package   # build the installable module package
```

The connection and delivery logic lives in `src/itach.js` and has no Companion dependency, so it's tested directly against the simulated device in `test/fake-itach.js`.

## Test builds

Every push builds an installable package (see `.github/workflows/test-package.yaml`). Each build is versioned `<version>-beta.<build number>`, so Companion always accepts it as a new version. Download it from:

- **Releases → "Test build"**: always the latest build (`.tgz` and a `.zip` with install notes)
- **Actions → Test Package → a run → Artifacts**: the build from any specific push, kept for 90 days
