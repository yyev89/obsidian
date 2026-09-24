## Best practices
### Performance:
1. Measure performance
2. Wait less
  - Minimize runner queuing
  - Fail fast
3. Do less
  - Conditional filters
  - Caching
4. Improve resource usage
  - Rapallelize (cores + jobs)
  - Avoid emulation (e.g. QUEMU)
5. Prevent slowdowns
### Caching
1. Git checkouts
2. Toolchains
3. Dependency downloads
4. Build/Test artifacts
5. Containers (images, layers, cache mounts)
### Maintanability
1. Mono vs Multi-repo considerations
2. Define a "CI API" for each service:
  - Use a single task runner or build tool (e.g. Task or Makefile)
  - Standardize a set of commands: install/test/build/dev
3. Avoid duplication by using Composite Actions and/or Reusable Workflows
4. Set up tooling to iterate locally (nektos/act)
### Security
1. Grant the minimum necessary permissions
2. Avoid long lived credentials where possible
3. Set up allowlist for specific approved actions
4. Pin action versions with the SHA
5. Don't allow self-hosted runners to execute on fork PRs
6. Require approval to run workflows using environments
