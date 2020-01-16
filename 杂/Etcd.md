# Etcd

- Open-source distributed key-value store.
- Pronounced as "et-cee-dee", making reference to distributing the Unix `/etc` directory.
- Most global configuration files live, across multiple machines at `/etc`.
- Serves as the backbone of many distributed systems, providing a reliable way for storing data across a cluster of servers.

## How does Etcd work?

Etcd is written in Go and uses the[ Raft protocol](http://raftconsensus.github.io/). Raft is a protocol for multiple nodes to maintain identical logs of state changing commands, and any node in a raft node may be treated as the master, and it will coordinate with the others to agree on which order state changes happen in.

Etcd’s job within Kubernetes is to safely store critical data for distributed systems. It’s best known as Kubernetes’ primary datastore used to store its configuration data, state, and metadata. Since