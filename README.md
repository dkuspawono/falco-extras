# Falco-extras


This repository contains [Falco](https://falco.org) container runtime security extras like security profiles for a growing list of the most popular container images. If you are using any image from the list below, the default ruleset will provide you with an initial set of security whitelists and blacklists (constraints) that you can extend to match your deployment or application specifics. Read more in our announcement blog post: [Implementing Docker/Kubernetes runtime security](https://sysdig.com/blog/docker-runtime-security/).

## What kind of unwanted behaviors can Falco detect?

Falco can detect and alert on any behavior that involves making Linux system calls, that means almost everything. For example, you can easily detect things like:

- A shell is run inside a container
- A server process spawns a child process of an unexpected type
- Unexpected read of a sensitive file (like /etc/shadow)
- A non-device file is written to /dev
- A standard system binary (like ls) makes an outbound network connection

## Images included in the runtime security profiles default ruleset

The current list of conatiner images included in the default ruleset is:

*   Kubernetes 1.10 cluster components:
    *   apiserver
    *   controller-manager
    *   kube-dns
    *   kube-scheduler
    *   dashboard
*   Google Kubernetes Engine components
*   Apache
*   Consul
*   ElasticSearch
*   etcd
*   Fluentd
*   HAproxy
*   MongoDB
*   Nginx
*   PHP-FPM
*   PostgreSQL
*   Redis
*   Traefik

# How to use these runtime security profiles

You can load custom rules for Falco using the [falco_rules.local.yaml](https://github.com/draios/falco/wiki/Falco-Rules#appending-to-lists-rules-and-macros) or the `rules.d` directory.

By default the rules are read in this order:

- /etc/falco/falco_rules.yaml
- /etc/falco/falco_rules.local.yaml
- /etc/falco/rules.d

For example, if you want to include the Traefik rules present in this repository, you can just drop the corresponding YAML file in the `rules.d` directory:

`cp rules/rules-traefik.yaml >> /etc/falco/falco_rules.d/`

If you are deploying Falco as a container, mount the path as an external volume where you can store the configration files or use a Kubernetes configMap. Then, restart he container.
If you are using Falco as a system service, restart the service.

**Note:** Take into account that these rules are based on strict whitelisting, and they have been created using default images. You will need to adapt them to your deployment to avoid false positives. Extend the rules whitelisting the paths your application use in the file system (caches, user data, additional libraries), specify other network activity like additional listing ports or network connections and any other running programs or binary utilities required during the lifecylce of your container (start, run and shutdown). You can read more about the Falco rule syntax in [Getting started writing Falco rules
](https://github.com/draios/falco/wiki/Falco-Rules) and more about Docker and Kubernetes security in our [Kubernetes Security Guide](https://sysdig.com/blog/kubernetes-security-guide/).

