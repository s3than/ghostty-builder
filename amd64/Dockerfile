FROM fedora AS stage1

ENV GHOSTTY_VERSION=1.0.1

WORKDIR /app

RUN dnf install -y zig gtk4-devel libadwaita-devel

RUN curl -L https://github.com/ghostty-org/ghostty/archive/refs/tags/v${GHOSTTY_VERSION}.tar.gz -o ghostty-v${GHOSTTY_VERSION}.tar.gz && \
    tar -xzf ghostty-v${GHOSTTY_VERSION}.tar.gz

RUN cd ghostty-${GHOSTTY_VERSION} && zig build -Doptimize=ReleaseFast

FROM scratch AS export-stage
ENV GHOSTTY_VERSION=1.0.1
COPY --from=stage1 /app/ghostty-${GHOSTTY_VERSION}/zig-out/bin/ghostty .
