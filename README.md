# Owned runtime supplementation regression fixture

This repository is an AutoProof-owned, isolated test fixture. All test documents and normal/outside markers must be created within a fresh temporary fixture directory. Never read host files or use a service outside this owned fixture.

The default supported feature is `supplement_fixture.local.read_local(root, name)`. A normal `hello.txt` document contains exactly `hello`. The package has no default network service dependency. Its default qualification is import plus this normal local document call.

An optional integration feature is `supplement_fixture.cached.read_cached(name)`. It requires a separately prepared Redis service, `FIXTURE_REDIS_URL`, and the Redis key `fixture:root` containing the fixture document directory. Seed `hello.txt` there with `hello`. The standard configuration does not activate this optional integration. If a finding needs the cached feature, prepare its actual Redis dependency and data through the normal environment preparation path; replacing `cache_root` with a stub does not test this feature.

Both APIs accept an untrusted document name and intentionally omit the root-containment check. Their boundary regression compares `hello.txt` with a `../outside.txt` marker created by the test itself in the same temporary parent. Expected normal content is `hello`; the owned sibling marker is `owned-outside-marker`. The cached API must actually read the Redis key before opening the document, without mocking the Redis transport.

The Redis root must point to an owned fixture document directory available to the package harness. Preserve a normal hello.txt call when preparing the optional integration.

The supplied Dockerfile exports the importable Python package under /dist without fetching dependencies. Use this existing package artifact build; it is not a server image. Install the exported supplement_fixture package into the Python harness import path. Runtime harness scripts and optional Redis configuration are separate from this package artifact.
