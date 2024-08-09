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
        specifier: ^1.0.0
        version: 1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)

packages:
  '@with-heart/pkg-a@1.0.0':
    resolution:
      {
        integrity: sha512-6aaHG4zUWjUaHw0ssh5jMijjs1P3sGqXU2U/LP6NWSc/QWOBG7hxjgq8KX0eh2LypV2WmkebWByfrcmuC4kxcQ==,
      }

  '@with-heart/pkg-b@1.0.0':
    resolution:
      {
        integrity: sha512-aY5X2+VnglmGXYUqNXL7JRy5c87PsqkOS1diRYgwh8m0CBmc0cXkkgrMRHkAMsL4vSS1EksxYaQuzWSVaoNw1A==,
      }

  '@with-heart/pkg-c@1.0.0':
    resolution:
      {
        integrity: sha512-gk5YMw2moEg6+R3x+JIZv4K71sQBbkZS+LrGxFPmluRIBjkujlffAmk4ydWIiftVpkSZlK9fnF4SZmKQCEIDHg==,
      }
    peerDependencies:
      '@with-heart/pkg-d': 1.0.0
      '@with-heart/pkg-e': ^1.0.0

  '@with-heart/pkg-d@1.0.0':
    resolution:
      {
        integrity: sha512-G1E8LX5Qs96ywRFSmtC5SDEJus/elOxuT8LQx6kPiFj1hH418EA8DCAvtWgX0SeynWzMWKKuQzIQTfDIAhvrPA==,
      }
    peerDependencies:
      '@with-heart/pkg-e': 1.0.0

  '@with-heart/pkg-e@1.0.0':
    resolution:
      {
        integrity: sha512-gEm1FFs47ODH90TMN0dCb/wSasAo6CLHcE3Uvz/lLIuquwGjIiS6NB51nAJ0vG3lJs73xYJmMiTiOSSBf/7vtA==,
      }

snapshots:
  '@with-heart/pkg-a@1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)':
    dependencies:
      '@with-heart/pkg-b': 1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-b@1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)':
    dependencies:
      '@with-heart/pkg-c': 1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)
    transitivePeerDependencies:
      - '@with-heart/pkg-d'
      - '@with-heart/pkg-e'

  '@with-heart/pkg-c@1.0.0(@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0))(@with-heart/pkg-e@1.0.0)':
    dependencies:
      '@with-heart/pkg-d': 1.0.0(@with-heart/pkg-e@1.0.0)
      '@with-heart/pkg-e': 1.0.0

  '@with-heart/pkg-d@1.0.0(@with-heart/pkg-e@1.0.0)':
    dependencies:
      '@with-heart/pkg-e': 1.0.0

  '@with-heart/pkg-e@1.0.0': {}
```
