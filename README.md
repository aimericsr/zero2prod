cargo install cargo-watch
cargo install cargo-audit
cargo install cargo-edit
cargo install cargo-expand
cargo install --version=0.5.7 sqlx-cli --no-default-features --features postgres
cargo install cargo-udeps
cargo +nightly udeps
cargo add actix-web@4.0.0
cargo add tokio@1.22.0 (tokio = { version = "1.22.0", features = ["macros", "rt-multi-thread"] })

cargo test
cargo tarpaulin --ignore-tests
cargo clippy -- -D warnings
cargo fmt -- --check
cargo audit

cargo watch -x check -x test -x run
rustup toolchain install nightly --allow-downgrade
cargo +nightly expand


sqlx migrate add mig1
SKIP_DOCKER=true ./scripts/init_db.sh

run certain tests : 
TEST_LOG=true cargo test health_check_works

docker build --tag zero2prod .
docker run -d --name rust-api -p 8000:8000 zero2prod

cargo sqlx prepare -- --lib

cargo build --tests

deploy(digital ocean) : 
brew install doctl
create read and write token
doctl auth init
doctl apps create --spec spec.yaml