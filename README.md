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
        specifier: ^1.0.2
        version: 1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)
      '@with-heart/pkg-d':
        specifier: ^1.0.2
        version: 1.0.2(@with-heart/pkg-e@1.0.2)
      '@with-heart/pkg-e':
        specifier: ^1.0.2
        version: 1.0.2

packages:
  '@with-heart/pkg-a@1.0.2':
    resolution:
      {
        integrity: sha512-mKVo/XOPI/bwN/TDd1pY/qERYb1dc4ub6vWRbceKDpdsvLLL5jETZJZqafXaXSE+2TPh4cdoMcmqS+e3ZCc53w==,
      }

  '@with-heart/pkg-b@1.0.2':
    resolution:
      {
        integrity: sha512-eoY5xgQtKUwarW8CSFORHT+YBVfmhl6vx+9zYYVj76pGLsUu7a6pNbZ9UQVTVv4s/3EOyTQpn192FhwHFYvqqQ==,
      }

  '@with-heart/pkg-c@1.0.2':
    resolution:
      {
        integrity: sha512-kl6JSo72K2JBkhOnjLTgPHKWlNGO223Yre5fBpF/euxvK5QvQrZfi9LOo5FcZxLqPi3AOWA9xo+yAH4vSbUyiw==,
      }
    peerDependencies:
      '@with-heart/pkg-d': 1.0.2
      '@with-heart/pkg-e': ^1.0.2

  '@with-heart/pkg-d@1.0.2':
    resolution:
      {
        integrity: sha512-en0PT9mwQtp3sFLovrwiS+86Jr5zaK91JT9Ngxc1ajWqVSN9koZ7Scxg4BR7mwAQHqTRO0K4QFqI4LNPf/pjUA==,
      }
    peerDependencies:
      '@with-heart/pkg-e': 1.0.2

  '@with-heart/pkg-e@1.0.2':
    resolution:
      {
        integrity: sha512-2WKWyAgUAzRQo+xCerM9Nl17k36Sacb/KQnD3eZY/MYSeGAFc23CHk7a341ILeeFAki9MzMSEjIL1L0nAluc9Q==,
      }

snapshots:
  '@with-heart/pkg-a@1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)':
    dependencies:
      '@with-heart/pkg-b': 1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-b@1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)':
    dependencies:
      '@with-heart/pkg-c': 1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-c@1.0.2(@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2))(@with-heart/pkg-e@1.0.2)':
    dependencies:
      '@with-heart/pkg-d': 1.0.2(@with-heart/pkg-e@1.0.2)
      '@with-heart/pkg-e': 1.0.2

  '@with-heart/pkg-d@1.0.2(@with-heart/pkg-e@1.0.2)':
    dependencies:
      '@with-heart/pkg-e': 1.0.2

  '@with-heart/pkg-e@1.0.2': {}
```
