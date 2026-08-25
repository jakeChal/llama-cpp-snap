## llama.cpp-snap

Fiddling with snap recipes on [`llama.cpp` project](https://github.com/ggml-org/llama.cpp)


### Tips and tricks

- To get the SHA from the release tarball: `sha256sum <path/to/tarball>`
- Generated snap is squashfs, so to inspect it: `unsquashfs -l <generated.snap>`
- In general, when setting-up the apps section and trying out a run of the app, 
if you see a shared lib failure:
    - check which lib it's complaining about, and add the Debian package for that lib to the `stage-packages` section
    - Retry a `snapcraft pack` --> `snap install ...` --> `<snapName> --help` cycle and if you still see issues with runtime libs:
        - `unsquashfs <nameOf.snap>`
        - `find squashfs-root -name "libMissing*"`
        - Adapt `LD_LIBRARY_PATH` in the `apps` section accordingly
- Setting up interfaces:
    - To verify the issue is related to a missing interface, check `jouranlctl`, e.g.
    `journalctl -xe | grep -i denied` and verify that the issue is AppArmor related