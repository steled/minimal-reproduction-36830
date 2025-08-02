# 36830

Reproduction for [Renovate issue 36830](https://github.com/renovatebot/renovate/discussions/36830).

## Current behavior

Renovate is throwing the following error in [action output](https://github.com/steled/argocd-apps/actions/runs/16665983010/job/47172390854#step:3:570).

```shell
DEBUG: Invalid manifest response (repository=steled/argocd-apps, baseBranch=main)
       "registry": "https://index.docker.io",
       "dockerRepository": "atmoz/sftp",
       "tag": "sha256:d8f111337a1b49755f8ca1c971c672e5baa1167c8332393dfa4cdf2a08bbacaa",
       "body": "",
       "headers": {
         "content-length": "1362",
         "content-type": "application/vnd.docker.distribution.manifest.v2+json",
         "docker-content-digest": "sha256:d8f111337a1b49755f8ca1c971c672e5baa1167c8332393dfa4cdf2a08bbacaa",
         "docker-distribution-api-version": "registry/2.0",
         "etag": "\"sha256:d8f111337a1b49755f8ca1c971c672e5baa1167c8332393dfa4cdf2a08bbacaa\"",
         "date": "Fri, 01 Aug 2025 04:04:38 GMT",
         "strict-transport-security": "max-age=31536000",
         "ratelimit-limit": "100;w=21600",
         "ratelimit-remaining": "36;w=21600",
         "docker-ratelimit-source": "52.176.19.194"
       },
       "err": {
         "message": "Schema error",
         "stack": "ZodError: Schema error\n    at Object.get error [as error] (/usr/local/renovate/node_modules/.pnpm/zod@3.25.76/node_modules/zod/v3/types.cjs:45:31)\n    at DockerDatasource.getManifest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:312:23)\n    at processTicksAndRejections (node:internal/process/task_queues:105:5)\n    at DockerDatasource.getConfigDigest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:278:8)\n    at DockerDatasource.getImageArchitecture (/usr/local/renovate/lib/modules/datasource/docker/index.ts:401:28)\n    at /usr/local/renovate/lib/util/cache/package/decorator.ts:131:19\n    at DockerDatasource.getDigest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:857:24)\n    at /usr/local/renovate/lib/util/cache/package/decorator.ts:131:19\n    at lookupUpdates (/usr/local/renovate/lib/workers/repository/process/lookup/index.ts:689:14)\n    at /usr/local/renovate/lib/workers/repository/process/fetch.ts:79:12\n    at Function.wrap (/usr/local/renovate/lib/util/stats.ts:38:20)\n    at /usr/local/renovate/lib/workers/repository/process/fetch.ts:125:23\n    at file:///usr/local/renovate/node_modules/.pnpm/p-map@6.0.0/node_modules/p-map/index.js:139:20",
         "issues": "Invalid JSON"
       }
```

## Expected behavior

### Running Github Action before version 41.0.21

Github Action runs wihtout errors

```shell
DEBUG: Dependency atmoz/sftp has unsupported/unversioned value alpine (versioning=docker) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getDigest(https://index.docker.io,/ atmoz/sftp, alpine) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getManifestResponse(https://index.docker.io,/ atmoz/sftp, sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f, head) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: http cache: saving https://auth.docker.io/token?scope=repository%3Aatmoz%2Fsftp%3Apull&service=registry.docker.io (etag=undefined, lastModified=undefined) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getManifestResponse(https://index.docker.io,/ atmoz/sftp, sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f, getText) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: Current digest sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f relates to architecture amd64 (repository=steled/argocd-apps, baseBranch=main)
DEBUG: Architecture-specific digest or missing docker-content-digest header - pulling full manifest (repository=steled/argocd-apps, baseBranch=main)
       "registryHost": "https://index.docker.io/",
       "dockerRepository": "atmoz/sftp"
DEBUG: getManifestResponse(https://index.docker.io,/ atmoz/sftp, alpine, getText) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: Got docker digest sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f (repository=steled/argocd-apps, baseBranch=main)
```

### Running Github Action after version 41.0.21

Githu Actions trows errors

```shell
DEBUG: Dependency atmoz/sftp has unsupported/unversioned value alpine (versioning=docker) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getDigest(https://index.docker.io,/ atmoz/sftp, alpine) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getManifestResponse(https://index.docker.io,/ atmoz/sftp, sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f, head) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: http cache: saving https://auth.docker.io/token?scope=repository%3Aatmoz%2Fsftp%3Apull&service=registry.docker.io (etag=undefined, lastModified=undefined) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: http cache: saving https://index.docker.io/v2/atmoz/sftp/manifests/sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f (etag="sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f", lastModified=undefined) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: getManifestResponse(https://index.docker.io,/ atmoz/sftp, sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f, getText) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: Invalid manifest response (repository=steled/argocd-apps, baseBranch=main)
       "registry": "https://index.docker.io/",
       "dockerRepository": "atmoz/sftp",
       "tag": "sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f",
       "body": "",
       "headers": {
         "content-length": "1362",
         "content-type": "application/vnd.docker.distribution.manifest.v2+json",
         "docker-content-digest": "sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f",
         "docker-distribution-api-version": "registry/2.0",
         "etag": "\"sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f\"",
         "date": "Thu, 01 May 2025 03:13:19 GMT",
         "strict-transport-security": "max-age=31[536](https://github.com/steled/argocd-apps/actions/runs/14768935391/job/41465536686#step:3:537)000",
         "ratelimit-limit": "100;w=21600",
         "ratelimit-remaining": "83;w=21600",
         "docker-ratelimit-source": "20.57.47.199"
       },
       "err": {
         "message": "Schema error",
         "stack": "ZodError: Schema error\n    at Object.get error [as error] (/usr/local/renovate/node_modules/.pnpm/zod@3.24.3/node_modules/zod/lib/types.js:55:31)\n    at DockerDatasource.getManifest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:311:23)\n    at processTicksAndRejections (node:internal/process/task_queues:105:5)\n    at DockerDatasource.getConfigDigest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:277:8)\n    at DockerDatasource.getImageArchitecture (/usr/local/renovate/lib/modules/datasource/docker/index.ts:400:28)\n    at /usr/local/renovate/lib/util/cache/package/decorator.ts:131:20\n    at DockerDatasource.getDigest (/usr/local/renovate/lib/modules/datasource/docker/index.ts:842:24)\n    at /usr/local/renovate/lib/util/cache/package/decorator.ts:131:20\n    at lookupUpdates (/usr/local/renovate/lib/workers/repository/process/lookup/index.ts:684:14)\n    at /usr/local/renovate/lib/workers/repository/process/fetch.ts:79:12\n    at Function.wrap (/usr/local/renovate/lib/util/stats.ts:38:20)\n    at /usr/local/renovate/lib/workers/repository/process/fetch.ts:125:23\n    at /usr/local/renovate/node_modules/.pnpm/p-map@4.0.0/node_modules/p-map/index.js:57:22",
         "issues": "Invalid JSON"
       }
DEBUG: getManifestResponse(https://index.docker.io, atmoz/sftp, alpine, head) (repository=steled/argocd-apps, baseBranch=main)
DEBUG: http cache: saving https://index.docker.io/v2/atmoz/sftp/manifests/alpine (etag="sha256:93d2d9c0e8e3d619e01321bd0eabaf746c9700115f92db5038d2d54b277ed22f", lastModified=undefined) (repository=steled/argocd-apps, baseBranch=main)
```
