# Pomerium for Kubernetes

Use Pomerium as a first-class secure-by-default Ingress Controller. The Pomerium Ingress Controller enables workflows more native to Kubernetes environments, such as Git-Ops style actions based on pull requests. Dynamically provision routes from Ingress resources and set policy based on annotations. By defining routes as Ingress resources you can independently create and remove them from Pomerium's configuration.

# Docs

- [Install Pomerium](https://www.pomerium.com/docs/k8s/install).
- [Global Configuration](https://www.pomerium.com/docs/k8s/configure).
- [Ingress Configuration](https://www.pomerium.com/docs/k8s/ingress).
- [Pomerium CRD Reference](https://www.pomerium.com/docs/k8s/reference).

# Fork releases

Push a version tag such as `v0.34.0` or `v0.34.0-svetoch.1` to run the
Release workflow. It compiles the controller and its Pomerium Core Go dependency
from source with `POMERIUM_OFFICIAL_BUILD=0`. Envoy is downloaded and embedded by
the existing build target; it is not compiled from source.

The workflow publishes:

- A GitHub release with Linux amd64 and arm64 archives containing the
  `ingress-controller` executable and Apache 2.0 license, plus `checksums.txt`.
- A multi-platform image at `ghcr.io/svetoch-dev/pomerium-ingress-controller:<tag>`
  (for example, `ghcr.io/svetoch-dev/pomerium-ingress-controller:v0.34.0`). Stable versions also receive
  the `latest` image tag; tags with a prerelease suffix create GitHub prereleases
  and do not update `latest`.

The image starts `/manager`; use `all-in-one` to run the controller together with
Pomerium Core, or `controller` to run only the Kubernetes controller.

Publishing uses the repository's automatic `GITHUB_TOKEN` with `contents: write`
and `packages: write`; no GCP, Cloudsmith, or upstream GitHub App secrets are
needed. GitHub Actions must be enabled for the fork and organization policy must
allow package publishing. New GHCR packages are private by default; change the
package visibility to public if anonymous image pulls are required.

Re-running a tag workflow replaces that release's assets and image tags. The
release version comes from the tag; the embedded Core version remains the one
pinned in `go.mod`.
