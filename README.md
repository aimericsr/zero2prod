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
doctl apps update 45cf1166-4edc-4413-84f4-7c5760e50f54 --spec=spec.yaml

migration : 
 DATABASE_URL=postgresql://newsletter:AVNS_KeL49HoqGcCx12CAbGe@app-e4530f83-add2-44d5-8bd6-fe11cda22d9d-do-user-13324123-0.b.db.ondigitalocean.com:25060/newsletter sqlx migrate run


step to update the database : 
- update the database with a nullable field
- update the api with a default value for the new field
- update the new column with non null constraints and a default value


export RUST_LOG="sqlx=error,info"
export TEST_LOG=enabled
cargo t subscribe_fails_if_there_is_a_fatal_database_error | bunyan


code coverage : cargo install grcov