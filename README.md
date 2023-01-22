Install rust toolchain : https://rustup.rs

rustc --version
cargo --version

cargo new my-app

faster linking(create file .cargo/config.toml) :
# On MacOS, `brew install michaeleisel/zld/zld`
[target.x86_64-apple-darwin]
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"]
[target.aarch64-apple-darwin]
rustflags = ["-C", "link-arg=-fuse-ld=/usr/local/bin/zld"]


cargo install cargo-watch --> cargo watch -x check -x test -x run
cargo install cargo-tarpaulin -f (code coverage, cargo tarpaulin --all-features --timeout 120 --output-dir test_reports --out Xml)
rustup component add clippy (linter, cargo clippy -- -D warnings)
rustup component add rustfmt (formatter, cargo fmt -- --check)
cargo install cargo-audit (scanning security vulnerabilities, cargo audit)
cargo deny, linter for dependencies
cargo install cargo-edit (needed to do cargo add some_crates)
cargo install cargo-expand (debug a macro like #[tokio::main])
install nightly compiler : rustup toolchain install nightly --allow-downgrade
cargo build --release --bin zero2prod 
cargo expand --bin zero2prod or cargo +nightly expand --bin zero2prod
cargo expand --test health_check
cargo install --version=0.5.7 sqlx-cli --no-default-features --features postgres
cargo install cargo-udeps (remove unused dependencies, cargo +nightly udeps)



migrate database : 
./scripts/init_db.sh
sqlx migrate add mig1
SKIP_DOCKER=true ./scripts/init_db.sh

run certain tests : 
TEST_LOG=true cargo test health_check_works
build binary for running test : cargo build --tests

run docker image : 
cargo sqlx prepare -- --lib
docker build --tag zero2prod .
docker run -d --name rust-api -p 8000:8000 zero2prod

deploy(digital ocean) : 
brew install doctl
create read and write token on the digital ocean website
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