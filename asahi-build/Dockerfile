FROM fedora AS stage1

ENV GHOSTTY_VERSION=1.0.1

WORKDIR /app

RUN dnf install -y zig gtk4-devel libadwaita-devel patch

# We override to dump in the PAGESIZE for the m2 mac running Asahi Linux
# https://github.com/ziglang/zig/blob/master/lib/std/mem.zig#L13
# https://github.com/AsahiLinux/docs/wiki/Broken-Software#broken-packages
# See https://github.com/ziglang/zig/issues/11308
COPY mem.zig.diff /usr/lib/zig/std/mem.zig.diff
RUN patch /usr/lib/zig/std/mem.zig < /usr/lib/zig/std/mem.zig.diff

RUN curl -L https://github.com/ghostty-org/ghostty/archive/refs/tags/v${GHOSTTY_VERSION}.tar.gz -o ghostty-v${GHOSTTY_VERSION}.tar.gz && \
    tar -xzf ghostty-v${GHOSTTY_VERSION}.tar.gz

# We dump in the build.zig file to override the arch for Asahi Linux
COPY build.zig.diff /app/ghostty-${GHOSTTY_VERSION}/build.zig.diff
RUN patch /app/ghostty-${GHOSTTY_VERSION}/build.zig < /app/ghostty-${GHOSTTY_VERSION}/build.zig.diff

RUN cd ghostty-${GHOSTTY_VERSION} && zig build -Doptimize=ReleaseFast

FROM scratch AS export-stage
ENV GHOSTTY_VERSION=1.0.1
COPY --from=stage1 /app/ghostty-${GHOSTTY_VERSION}/zig-out/bin/ghostty .
