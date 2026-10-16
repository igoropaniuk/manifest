# Repo manifest for OP-TEE development

In the OP-TEE project we try to gather all documentation under the
[optee_os](https://github.com/OP-TEE/optee_os) git, therefore we recommend that
you read the instructions at
[this](https://github.com/OP-TEE/optee_os/blob/master/README.md) page which
describes how to setup OP-TEE on various platforms.

## Arduino UNO Q / Agatti (QRB2210, QCM2290)

`agatti-uno-q.xml` on the `agatti` branch pins an open boot chain for the
Arduino UNO Q: TF-A (uno_q), OP-TEE (qcom-agatti, with the ADSP PAS
pseudo-TA), U-Boot, mainline Linux v7.3-rc6 and a Buildroot rootfs, driven
by `build/agatti.mk`.

```sh
mkdir agatti && cd agatti
repo init -u https://github.com/igoropaniuk/manifest.git -b agatti -m agatti-uno-q.xml
repo sync -c
make -C build aarch64-toolchain   # ARM GNU 14.3 into toolchains/
make -C build all                 # bootimage + linux + buildroot + efi + firmware
make -C build flash-full          # board on a Bughopper, EDL via pytactl + qdl
```

`make -C build help` lists the targets. Everything that is not source is
fetched by the build: the ARM toolchain, the stock QRB2210 boot binaries
(Qualcomm Software Center release 00021, sha256-pinned), the Agatti
security profile (qualcomm/security-profiles) and the ADSP image
(linux-firmware). Not fetched: Qualcomm sectools (expected at
`sectools/latest/Linux_<arch>/sectools`, needed to CASS-sign BL2), `qdl`
and `pytactl` for flashing.
