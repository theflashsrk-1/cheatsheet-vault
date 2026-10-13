# Containers

Container and orchestrator attack surface: breaking out of Docker containers to host root, and abusing Kubernetes RBAC to escalate from a pod to the underlying node.

| Note | Covers |
|---|---|
| [docker](docker.md) | Container-context checks, host-side enumeration, and escape via exposed socket, `--privileged`, dangerous capabilities, exposed remote API, or a writable host mount |
| [kubernetes](kubernetes.md) | kubectl/ServiceAccount-token enumeration, `auth can-i` permission checks, dangerous RBAC primitives, secrets harvesting, and pod → node escape |
