# Awesome Java Self-Hosting

An opinionated, actively maintained collection of resources, tools, and practices for self-hosting Java and JVM applications.

The list focuses on practical deployment and operations, especially for small servers, VPSes, and resource-conscious environments.

This is a curated list, not an exhaustive directory. Simple, maintainable, and proportional infrastructure is preferred over adding components by default.

The advice applies to Spring Boot, Quarkus, Micronaut, plain Java services, and other JVM applications where relevant.

## Philosophy

- **Simple infrastructure by default.** Prefer fewer moving parts and proportional solutions.
- **One strong recommendation is better than ten similar alternatives.**
- **Production usefulness over popularity.**
- **Prefer maintained projects and authoritative documentation.**
- **Small and resource-constrained deployments are first-class use cases.**
- Larger conventional tools belong when they solve a real need; a small service does not automatically need a platform stack.
- Categories are intentionally selective. The list is not a provider directory or a catalog of Java-written software.

## Spring Boot deployment

- [Deploying Spring Boot applications](https://docs.spring.io/spring-boot/how-to/deployment/) - Official overview of executable archives, services, containers, and other deployment choices.

## Java runtimes

- [Eclipse Temurin](https://adoptium.net/temurin/) - A sensible general-purpose OpenJDK distribution with documented support and regular releases.
- [Java launcher and JVM options](https://docs.oracle.com/en/java/javase/25/docs/specs/man/java.html) - Reference for container support, heap sizing, diagnostics, and runtime options; check the matching documentation for the JDK version you deploy.

## Packaging

- [Spring Boot Cloud Native Buildpacks](https://docs.spring.io/spring-boot/reference/packaging/container-images/cloud-native-buildpacks.html) - Build OCI images without maintaining a Dockerfile for straightforward container deployments.
- [Docker multi-stage builds](https://docs.docker.com/build/building/multi-stage/) - Keep build tooling out of the final image when you need a custom container image.
- [jlink](https://docs.oracle.com/en/java/javase/25/docs/specs/man/jlink.html) - Create a smaller custom runtime image when you can own and validate the resulting module set.

Containers and native images have real operational tradeoffs. Use them when they simplify delivery, improve density, or meet a concrete startup or footprint requirement; an executable JAR is often the easier first deployment.

## Running Java as a service

- [systemd service units](https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html) - Configure startup, restart behavior, dependencies, and graceful process shutdown on common Linux servers.
- [systemd execution environment](https://www.freedesktop.org/software/systemd/man/latest/systemd.exec.html) - Set a dedicated user, working directory, resource limits, environment, and filesystem protections for a service.
- [Spring Boot graceful shutdown](https://docs.spring.io/spring-boot/reference/web/graceful-shutdown.html) - Let requests finish cleanly during a service restart or deployment.

## Containers

- [Docker Compose](https://docs.docker.com/compose/) - Define a small multi-container application and its persistent services in one readable configuration.

## Reverse proxy and TLS

- [Caddy automatic HTTPS](https://caddyserver.com/docs/automatic-https) - Serve a public application with automatic certificate issuance and renewal using a compact configuration.
- [nginx reverse proxy guide](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) - A conventional option when nginx is already part of the host setup or its routing controls are needed.
- [Spring Boot forwarded headers](https://docs.spring.io/spring-boot/how-to/webserver.html) - Configure the application to interpret client scheme and address correctly behind a trusted proxy.

## Configuration and secrets

- [Spring Boot externalized configuration](https://docs.spring.io/spring-boot/reference/features/external-config.html) - Keep deployment-specific settings outside the packaged application.

## Databases, persistence, and backups

- [PostgreSQL backup and restore](https://www.postgresql.org/docs/current/backup.html) - Official guidance for SQL dumps, filesystem-level backups, and continuous archiving; choose a method and practice its restore path.
- [SQLite: when to use SQLite](https://www.sqlite.org/whentouse.html) - Understand when a local embedded database is a good fit and when a client/server database is more appropriate.
- [Flyway](https://documentation.red-gate.com/flyway) - Track schema changes as repeatable migrations that can be applied during deployment.
- [restic](https://restic.net/) and its [restore guide](https://restic.readthedocs.io/en/stable/050_restore.html) - Encrypted, deduplicated backups to local or remote storage, with documented integrity checks and recovery steps.

Persist database files and uploads deliberately, including when using containers. Keep at least one backup off the application host and rehearse recovery.

## Metrics, health, and monitoring

- [Spring Boot Actuator](https://docs.spring.io/spring-boot/reference/actuator/index.html) - The production health and management foundation for Spring Boot; expose only the endpoints required by operators and monitoring.
- [Micrometer](https://docs.micrometer.io/micrometer/reference/index.html) - Instrument JVM and application behavior through a metrics facade with registry choices for common monitoring systems.
- [Prometheus](https://prometheus.io/docs/introduction/overview/) and [Grafana](https://grafana.com/docs/grafana/latest/) - A flexible, extensible metrics and dashboard stack when retention, alerting, integrations, or multiple hosts justify it.
- [Prometheus Node Exporter](https://prometheus.io/docs/guides/node-exporter/) - Export Linux host metrics such as CPU, memory, filesystem, and network data for Prometheus.
- [Uptime Kuma](https://github.com/louislam/uptime-kuma) - Self-hosted HTTP, TCP, DNS, and other uptime checks with a straightforward status dashboard.
- [StatLite](https://github.com/PVRLabs/statlite) - Lightweight self-hosted monitoring for a small number of Java applications and hosts, with local SQLite storage and support for Spring Boot metrics.
- [Lightweight Spring Boot Monitoring Without Prometheus and Grafana](https://pvrlabs.xyz/articles/lightweight-spring-boot-monitoring.html) - A practical walkthrough of monitoring Spring Boot Actuator metrics on a small self-managed host.

For Spring Boot, Micrometer registry exports are generally the production collection path; Actuator's generic metrics endpoint is useful for inspection and diagnosis.

## JVM diagnostics and performance

- [Java Flight Recorder operations](https://docs.oracle.com/en/java/javase/27/troubleshoot/diagnostic-tools.html) - Start and manage recordings with startup options or `jcmd`, then inspect them with `jfr` or JDK Mission Control when diagnosing production behavior.
- [Java Mission Control](https://www.oracle.com/java/technologies/jdk-mission-control.html) - Analyze Flight Recorder recordings and inspect JVM behavior with a desktop tool.
- [JDK serviceability tools](https://docs.oracle.com/en/java/javase/25/docs/specs/man/index.html) - Use tools such as `jcmd`, `jstack`, and `jmap` from a matching JDK to inspect a running process or capture evidence during an incident.
- [async-profiler](https://github.com/async-profiler/async-profiler) - Profile CPU, allocations, native memory, locks, and other events with low overhead when JFR or sampling needs a deeper view.
- [Native Memory Tracking](https://docs.oracle.com/en/java/javase/25/vm/native-memory-tracking.html) - Use NMT with `jcmd` to account for JVM-managed native memory when heap size does not explain process memory.

Measure before tuning. Capture workload, CPU, latency, garbage collection, heap, and resident memory data before changing JVM flags or adding capacity.

## Logging

- [Spring Boot logging](https://docs.spring.io/spring-boot/reference/features/logging.html) - Configure console output, log levels, and structured formats using built-in support.
- [systemd journal](https://www.freedesktop.org/software/systemd/man/latest/journald.html) - Collect service stdout and stderr in the host journal and inspect them with `journalctl` before adding a separate logging stack.

## Security

- [Spring Boot Actuator security](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html) - Understand endpoint exposure and access controls so management routes stay within the intended trust boundary.
- [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) - Apply practical principles for storing, accessing, rotating, and auditing application secrets.
- [Ubuntu firewall guide](https://documentation.ubuntu.com/server/how-to/security/firewalls/) - Limit network exposure with host firewall rules; allow only required public ports and keep application ports private behind the proxy.

## Deployment automation

- [GitHub Actions](https://docs.github.com/en/actions) - Build a tested artifact or image, then use a narrowly scoped SSH deployment job to copy the release and restart a service.

## Small-server efficiency

- [Spring Boot memory usage experiments](https://github.com/dsyer/spring-boot-memory-blog) - Measured experiments on Spring application memory, including heap, native memory, container limits, and dependency choices; treat older measurements as methodology, not current sizing promises.
- [PVR Labs small-server experiments](https://github.com/PVRLabs/experiments/tree/main/statlite/tierhive-recipe-256mb) - A reproducible Java-on-a-small-VPS deployment experiment with a 256 MiB host and measured application and monitoring behavior.
- [Compact Object Headers in the JDK 27 Java command reference](https://docs.oracle.com/en/java/javase/27/docs/specs/man/java.html) - They reduce heap footprint; they are enabled by default in JDK 27, while JDK 25 requires `-XX:+UseCompactObjectHeaders`. Benchmark the behavior on your target JDK and workload before overriding the default.

Heap is only part of process memory. Include metaspace, code cache, thread stacks, direct buffers, native libraries, and other processes when sizing a host or container.

## Framework operations

- **Spring Boot:** [production-ready features](https://docs.spring.io/spring-boot/reference/actuator/index.html) and [container image packaging](https://docs.spring.io/spring-boot/reference/packaging/container-images/index.html) cover common production health, metrics, and image workflows. For broader Spring Boot development tools, see [Awesome Modern Spring Boot](https://github.com/ejboy/awesome-modern-spring-boot).
- **Quarkus:** [Packaging overview](https://quarkus.io/guides/packaging) explains production packaging, including the default fast-jar output and the full directory that must be deployed with it.
- **Micronaut:** [Micronaut deployment](https://docs.micronaut.io/latest/guide/#deployment) describes packaging and running Micronaut applications in production environments.

## Practices

- Keep infrastructure proportional to the application and its recovery needs.
- Prefer a boring deployment path that the operator can understand and restore.
- Run each application as a dedicated, unprivileged service user.
- Terminate TLS at a clear, maintained boundary and trust forwarded headers only from that proxy.
- Back up data off-server and test a restore.
- Expose management endpoints intentionally.
- Measure before tuning, and account for total JVM process memory rather than only `-Xmx`.
- Keep the operating system and JDK patched.
- Start simple and add infrastructure when an actual requirement appears.

## Contributing

Self-submissions are welcome. This is an editorial, intentionally incomplete list; absence is not a negative judgment. Please disclose affiliations and see [CONTRIBUTING.md](CONTRIBUTING.md) before proposing a resource.

## Related

- [Awesome Modern Spring Boot](https://github.com/ejboy/awesome-modern-spring-boot) - Broader curated coverage of modern Spring Boot development tools and practices.
- [Awesome Efficient Devtools](https://github.com/ejboy/awesome-efficient-devtools) - Tools and practices for efficient development workflows.

## Maintainer note

The maintainer develops some listed projects through [PVR Labs](https://pvrlabs.xyz/). Those projects are evaluated under the same editorial criteria as every other resource in this list.
