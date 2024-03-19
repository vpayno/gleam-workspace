# gleam-workspace

Gleam workspace.

## Gleam Installation

Note: you can use [RunMe](https://github.com/stateful/runme) to use this readme as a playbook.

### ERLang

Install or update ERLang:

```bash { background=false category=setup closeTerminalOnSuccess=true excludeFromRunAll=true interactive=true interpreter=bash name=install-erlang promptEnv=true terminalRows=10 }
set -e

if [[ ! -d ~/git_remote/erlang-otp ]]; then
    git clone https://github.com/erlang/otp ~/git_remote/erlang-otp
    cd ~/git_remote/erlang-otp
else
    cd ~/git_remote/erlang-otp
    git switch master
    git pull
    git remote prune origin
    git gc --auto
fi

declare tag_ver
tag_ver="$(git tag | grep -E '^OTP-[0-9]+\.' | grep -v -- rc[0-9] | tail -n 1)"

git switch --detach "${tag_ver}"

time ./configure --prefix="${HOME}/.local/erlang/v26"
time make
time make install
printf "\n"

if ! grep -q erlang/"${tag_ver%%.*}" ~/.bash_libs.d/46-erlang.bash; then
    cat >> ~/.bash_libs.d/46-erlang.bash <<-EOF
    export PATH="${HOME}/.local/erlang/${tag_ver%%.*}/bin:\${PATH}"
EOF
fi

source ~/.bash_libs.d/46-erlang.bash

printf "\n" | erl
```

### Rebar3

Install or update Rebar3:

```bash { background=false category=setup closeTerminalOnSuccess=true excludeFromRunAll=true interactive=true interpreter=bash name=install-rebar3 promptEnv=true terminalRows=10 }
set -e

if [[ ! -d ~/git_remote/rebar3 ]]; then
    git clone https://github.com/erlang/rebar3 ~/git_remote/rebar3
    cd ~/git_remote/rebar3
else
    cd ~/git_remote/rebar3
    git switch main
    git pull
    git remote prune origin
    git gc --auto
fi

declare tag_ver
tag_ver="$(git tag | grep -E '^[0-9]+\.' | grep -v -- rc[0-9] | tail -n 1)"

git switch --detach "${tag_ver}"

time ./bootstrap
time ./rebar3 local install
printf "\n"

if ! grep -q .cache/rebar3/bin ~/.bash_libs.d/46-erlang.bash; then
    cat >> ~/.bash_libs.d/46-erlang.bash <<-EOF
    export PATH="${HOME}/.cache/rebar3/bin:\${PATH}"
EOF

source ~/.bash_libs.d/46-erlang.bash

rebar3 --version
fi
```

### Gleam

Install or update Gleam:

```bash { background=false category=setup closeTerminalOnSuccess=true excludeFromRunAll=true interactive=true interpreter=bash name=install-gleam promptEnv=true terminalRows=10 }
set -e

if [[ ! -d ~/git_remote/gleam-lang ]]; then
    git clone https://github.com/gleam-lang/gleam ~/git_remote/gleam-lang
    cd ~/git_remote/gleam-lang
else
    cd ~/git_remote/gleam-lang
    git switch main
    git pull
    git remote prune origin
    git gc --auto
fi

declare tag_ver
tag_ver="$(git tag | grep -E '^v[0-9]+\.' | grep -v -- -rc[0-9] | tail -n 1)"

git switch --detach "${tag_ver}"

time make install
printf "\n"

gleam --version
```
