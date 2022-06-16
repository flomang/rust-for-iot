FROM rust:1.56.0 AS chef 
# We only pay the installation cost once, 
# it will be cached from the second build onwards
RUN cargo install cargo-chef 
WORKDIR app

FROM chef AS planner
COPY . .
RUN cargo chef prepare  --recipe-path recipe.json

FROM chef AS builder

WORKDIR /home/rust/src

COPY --from=planner /app/recipe.json recipe.json
# Build dependencies - this is the caching Docker layer!
RUN cargo chef cook --release --features vendored --recipe-path recipe.json

# Build application
COPY . .
RUN cargo build --release --features vendored 

#RUN echo $(ls -l /home/rust/src/target/release)
# We do not need the Rust toolchain to run the binary!
FROM debian:buster-slim AS runtime
WORKDIR app
COPY --from=builder /home/rust/src/target/release/upload_svc .

ENTRYPOINT ["/app/upload_svc"]