Sets up some dependencies using the `workspace:` protocol in order to test
`changesets` + `pnpm publish`.

# Dependencies

Dependencies specified by `packages/*/package.json`:

```yaml
'@with-heart/pkg-a':
  dependencies:
    '@with-heart/pkg-b': 'workspace:^'
'@with-heart/pkg-b':
  dependencies:
    '@with-heart/pkg-c': 'workspace:^'
'@with-heart/pkg-c':
  peerDependencies:
    '@with-heart/pkg-d': 'workspace:*'
    '@with-heart/pkg-e': 'workspace:^'
'@with-heart/pkg-d':
  peerDependencies:
    '@with-heart/pkg-e': 'workspace:*'
'@with-heart/pkg-e':
```

When installing `@with-heart/pkg-a` as a dependency in a package, we get this
`pnpm-lock.yaml`:

```yaml
lockfileVersion: '9.0'

settings:
  autoInstallPeers: true
  excludeLinksFromLockfile: false

importers:
  .:
    dependencies:
      '@with-heart/pkg-a':
        specifier: ^1.0.6
        version: 1.0.6(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)

packages:
  '@with-heart/pkg-a@1.0.6':
    resolution:
      {
        integrity: sha512-sL4IcMEcOU3/gNFpabRZINAGRGi4U4ioBmmvWavcuJuF8v2uQLFVer1zyNIZwozGnVvlY8yRl0VYY1303rF8bg==,
      }

  '@with-heart/pkg-b@2.0.1':
    resolution:
      {
        integrity: sha512-xS9CDe/4P3Avfrpm40addSlj0yAyig8HQfuBHpKPE9TAJjn0MmIsPrZJKzToahhvvQ6egbgnxt6Iuub+JxM7nQ==,
      }

  '@with-heart/pkg-c@4.0.0':
    resolution:
      {
        integrity: sha512-HqVwClREaX5xUN2OnLxdPYNysJbjXWEBakH2h6QMWs2xAPyLgt7dOWeh09RnkORpk++bI72Yn0oJypBPxsBAzg==,
      }
    peerDependencies:
      '@with-heart/pkg-d': 3.0.0
      '@with-heart/pkg-e': ^3.0.0

  '@with-heart/pkg-d@3.0.0':
    resolution:
      {
        integrity: sha512-c1mZ4bVALchzZKNCwWNifEwM1Q1/lTQOFASep80J9QTNfwoR3LS+WKcLCXmIJQMagb9TRluIqRBjIQXiA4+QSw==,
      }
    peerDependencies:
      '@with-heart/pkg-e': 3.0.0

  '@with-heart/pkg-e@3.0.0':
    resolution:
      {
        integrity: sha512-S566B3Pj+rc5Xw6i1C6gS3jw1VGrU1Rqhl77J1IcioC4AYTtrJrT+k4ixgSFDVa4sLUnpHsJiGn0cf1wV3mbfA==,
      }

snapshots:
  '@with-heart/pkg-a@1.0.6(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)':
    dependencies:
      '@with-heart/pkg-b': 2.0.1(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-b@2.0.1(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)':
    dependencies:
      '@with-heart/pkg-c': 4.0.0(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-c@4.0.0(@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0))(@with-heart/pkg-e@3.0.0)':
    dependencies:
      '@with-heart/pkg-d': 3.0.0(@with-heart/pkg-e@3.0.0)
      '@with-heart/pkg-e': 3.0.0

  '@with-heart/pkg-d@3.0.0(@with-heart/pkg-e@3.0.0)':
    dependencies:
      '@with-heart/pkg-e': 3.0.0

  '@with-heart/pkg-e@3.0.0': {}
```
