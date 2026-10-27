# Origin Cloud API

The protobuf definitions of Origin Cloud's public REST APIs. Each service's API
is defined here first and served as REST/JSON at `{service}.api.origincloud.eu`.
Origin Cloud's SDKs, Terraform provider, CLI and MCP server are generated from
these files.

| Package | Service | Hostname |
|---|---|---|
| `origincloud.pvs.v1beta1` | Persistent volumes: volumes, snapshots, storage classes | `pvs.api.origincloud.eu` |
| `origincloud.longrunning.v1beta1` | Operations: the state of an asynchronous change | the hostname of the service that started it |

## Design

The APIs follow Google's [API Improvement Proposals](https://google.aip.dev)
(AIP), with these deviations:

- **Resource names carry bare ids** under their parent
  (`projects/{project}/zones/{zone}/volumes/{volume}`). Every resource also has
  a `urn`, its identifier across services and in access policies.
- **Lists page by an opaque `page_token`.**
- **Errors are `google.rpc.Status`** in the JSON body, with
  `google.rpc.ErrorInfo` naming the reason.
- **Asynchronous changes return an operation** (AIP-151), which a client reads
  or waits on through `origincloud.longrunning`. A create takes a `request_id`
  (AIP-155): a repeat with the same id returns the first request's operation.
- **A write answers with an `Origin-Consistency-Token` header.** A read that
  presents it sees that write.

## Versions

The version is part of the package and the path: `origincloud.pvs.v1beta1` is
served under `/v1beta1/`.

- **A preview version** (`v1alpha1`, `v1beta1`) may change in incompatible ways.
- **A stable version** (`v1`) never does. A change that would break it ships as
  a new major version instead.

## Using the definitions

The repository is a [buf](https://buf.build) module that depends on
`buf.build/googleapis/googleapis`. Generate code from it as a git input:

```sh
buf generate https://github.com/origincloudhq/api.git --template buf.gen.yaml
```

## Contributing

Issues are welcome. This repository is published from Origin Cloud's own
source, so a pull request here is not merged; see [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache License 2.0; see [LICENSE](LICENSE).
