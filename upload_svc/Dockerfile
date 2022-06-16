#FROM ekidd/rust-musl-builder:1.57.0 as build
FROM messense/rust-musl-cross:aarch64-musl as build
#FROM messense/rust-musl-cross:x86_64-musl as build
#FROM rust:slim

ADD --chown=rust:rust . ./

RUN cargo build --release --features vendored

FROM scratch

##COPY --from=build /home/rust/src/target/x86_64-unknown-linux-musl/release/upload_svc .
##COPY --from=build /home/rust/src/target/arm-unknown-linux-musleabi/release/upload_svc .
COPY --from=build /home/rust/src/target/aarch64-unknown-linux-musl/release/upload_svc .

EXPOSE 3001

CMD ["/upload_svc"]