FROM ubuntu:20.04

# The dockerfile closely follows the "linux" instructions from the 330 project 0
# README

# Mostly so apt does not require interaction
ENV TZ=America/New_York
RUN apt-get update
RUN apt-get install -y tzdata

# 330 dependencies
RUN apt-get install -y ruby-dev ocaml ocaml-native-compilers camlp4 make m4 curl graphviz libssl-dev pkg-config
RUN gem install --no-document minitest

# OCamel package management installation
RUN apt-get install -y opam
RUN opam init --disable-sandboxing
RUN echo 'eval `opam config env`' >> ~/.profile
# Just so we capture that last eval statement
RUN . ~/.profile

# And OCamel package installation using opam
RUN opam update
RUN opam switch create 4.13.0
RUN eval $(opam env)
RUN opam install -y ocamlfind ounit utop dune qcheck

# Rust installation
# The ... -s -- -y is slightly hackish since -s tells curl to read from
# standard input and -- -y tells bash to then give -y to whatever sh is
# running, which in this case will be the rust installation script.
# Effectively, the rust install then becomes noninteractive. In my defense,
# the rust github actually recommended this method in an issue.
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
RUN echo "export PATH=\"$HOME/.cargo/bin:$PATH\"" >> ~/.bashrc

# And make sure we can actually submit the projects (bashrc to update path)
RUN . ~/.bashrc && cargo install gradescope-submit

# ruby linting support (see the wiki section on vscode setup)
RUN gem install ruby-debug-ide debase rubocop solargraph

# The point of the container is to test things, so by default run a shell
CMD bash --login